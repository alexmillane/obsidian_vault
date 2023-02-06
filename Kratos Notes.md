#kratos #ngc #kibana


## Lionel's Magic Script
I have not tested this yet but it will likely be useful.

```
#!/usr/bin/python3
"""
Copyright (c) 2021-2022, NVIDIA CORPORATION. All rights reserved.
NVIDIA CORPORATION and its licensors retain all intellectual property
and proprietary rights in and to this software, related documentation
and any modifications thereto. Any use, reproduction, disclosure or
distribution of this software and related documentation without an express
license agreement from NVIDIA CORPORATION is strictly prohibited.
"""
"""
This script is for pushing KPI payloads defined in JSON files to Kratos.
Example usage:
push_to_kratos --file kpis.json --index egm_train
"""
import argparse
import logging
import json
import os
from pymeasure import Domain, Measurement, MeasurementClient, Resource

KRATOS_DEV_SERVER = 'prod.analytics.nvidiagrid.net'

def upload_KPIs(args, additional_data):
    """
    Appends additional data as (key, value) pairs to payload.
    Uploads contents of KPI file to Kratos at the given KPI index.
    Args:
        args: parameters for KPI JSON file & KPI index
        additional_data: extra (key, value) pairs added to payload (i.e., workflow_id, output_data)
    """
    file = args.file
    index = args.index
    # check if file exists
    if not os.path.isfile(file):
        print('file:', file, 'not found')
        return
    # read-in file
    with open(file, 'r') as f:
        data = json.load(f)
        if args.is_summary_kpis:
            # process input data specific to summary KPIs use-case
            payloads = data['workflows']
        else:
            payloads = data if type(data) is list else [data]
    print('uploading', str(file), 'to Kratos', '\n')
    # upload each payload
    for i, payload in enumerate(payloads):
        index = payload.get('kpi_index', index)
        if index is None:
            index = args.index
        # add additional data to payload
        for key, value in additional_data:
            payload[key] = value
        print(f'\n ---- payload to index {index} for', (i+1), '---- \n')
        for key in payload:
            val = payload[key]
            print(key, ':', val)
        print()
        kratos = KratosManager(index)
        kratos.push_payload(payload)

def process_arg_list(arg_list):
    """
    Validate and process additional args
    Args:
        arg_list: list of additional cmd line args
    Raises:
        ValueError: Number of args is not even, or arg names do not prefix with '--'
    Returns
        List of (key, value) pairs to be added to KPI JSON payload, such as [("workflow_id", 123)]
    """
    if len(arg_list) % 2 != 0:
        raise ValueError('num of cmd line args must be even, '
                         'since each arg name requires an arg value')
    for i, elem in enumerate(arg_list):
        if i % 2 == 0:
            if not elem.startswith('--'):
                raise ValueError('arg name: ', elem, ' should be prefixed with "--"')
    it = iter(arg_list)
    return [(e[2:], next(it)) for e in it]

class KratosManager:
    """ Pushes payloads to Kratos for a given index """
    def __init__(self, index_postfix):
        """
        Initializes MeasurementClient & Domain
        Args:
            index_postfix: the suffix to the full KPI index
        """
        self.measurand_name = 'KPIResult'
        self.kfw_client = MeasurementClient(KRATOS_DEV_SERVER)
        self.domain = Domain('robotics', 'index', 'robotics.' + index_postfix)
    def add_resources(self, payload, measurement):
        """
        Add each (key, value) from payload data structure to measurement
        Args:
            payload: KPI data structure
            measurement: the Kratos measurement
        """
        for key, value in payload.items():
            if isinstance(value, dict):
                self.add_resources(value, measurement)
            elif isinstance(value, list):
                json_val = json.dumps(value, default=str)
                # Force JSON type objects to be intepreted as
                # text values to avoid dropping KPI payloads due to
                # field type mismatch errors in Elastic Search
                stringify_json = "Str: " + json_val
                measurement.add_resource(Resource(key, stringify_json))
            else:
                measurement.add_resource(Resource(key, value))
    def push_payload(self, payload):
        """
        Upload KPI file to Kratos
        Args:
            payload: JSON with payload
        """
        measurement = Measurement(self.domain, measurand=self.measurand_name)
        self.add_resources(payload, measurement)
        self.kfw_client.send(measurement)
        self.kfw_client.shutdown()

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument('--file', type=str, required=True)
    parser.add_argument('--index', type=str, required=True)
    parser.add_argument('--is_summary_kpis', action='store_true',
                        help='flag to process input data specific to summary KPIs use-case')
    args, additional_data = parser.parse_known_args()
    additional_data = process_arg_list(additional_data)
    # Set-up logging for Pymeasure
    logging.basicConfig(format='%(asctime)s %(levelname)s %(name)s - %(message)s',
                        level=logging.INFO)
    upload_KPIs(args, additional_data)
```