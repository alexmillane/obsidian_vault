


Compress the asset
```
tar -czvf quickstart.tar.gz quickstart/
```


Upload the asset to NGC
```
ngc registry resource upload-version nvstaging/isaac/isaac_ros_assets:isaac_perceptor --source /home/alex/datasets/perceptor_quickstart/quickstart.tar.gz
```
