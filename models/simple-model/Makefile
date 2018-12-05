LOCAL_SERVICE_PORT=5500
IMAGE_NAME=127.0.0.1:5000/gsunner/simple-model
MODEL_VERSION=v0

watch_docker:
	@watch -n 1 'docker ps -a'

build_model_image:
	s2i build . seldonio/seldon-core-s2i-python2 $(IMAGE_NAME):$(MODEL_VERSION)

push_to_registry:
	docker push $(IMAGE_NAME):$(MODEL_VERSION)

start_serving_model:
	docker run --name=simple-model-service -d -p $(LOCAL_SERVICE_PORT):5000 $(IMAGE_NAME):$(MODEL_VERSION)

stop_serving_model:
	docker rm -v -f simple-model-service

test_predict_call:
	curl -H "Content-Type: application/x-www-form-urlencoded" -d 'json={"data":{"names":["a"],"tensor":{"shape":[1,1],"values":[0]}}}' "http://127.0.0.1:$(LOCAL_SERVICE_PORT)/predict"

build_model_image_using_docker:
	docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock -v $$PWD:/work gsunner/s2i-tools:0.1 sh -c 's2i build . seldonio/seldon-core-s2i-python2 $(IMAGE_NAME):$(MODEL_VERSION)'

