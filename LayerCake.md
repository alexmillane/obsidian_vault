#nvblox #corelib #design
Some ideas brainstorm the design of LayerCake.
```
class Mapper {
 public:
 	Mapper() {
	  tsdf_layer_index_ = cake_.add_layer<TsdfLayer>();
	}

	TsdfLayer& tsdf_layer() {
		return cake_.get<TsdfLayer>(tsdf_layer_index_);
	}

 private:

	LayerCake cake_;
	unsigned int tsdf_layer_index_;
};

class LayerCake {
 public:

	LayerCake(float voxel_size);

	template< typename LayerType>
	unsigned int add_layer();

  
	template< typename LayerType>
	LayerType& get_layer(unsigned int layer_index) {
		std::dynamic_cast<LayerType>(layers.find(layer_index));
		// Do some checking that this worked.
	}

 private:

	// Params
	float voxel_size_ = 0.0f;

	// Stored layers
	std::unordered_map<unsigned int, std::unique_ptr<BaseLayer>> layers_;
};
```
