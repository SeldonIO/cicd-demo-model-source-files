LOCAL_SERVICE_PORT=5500
REGISTRY_HOST=127.0.0.1
REGISTRY_PORT=5000
IMAGE_ORG=gsunner
IMAGE_SHORT_NAME=simple-model
MODEL_VERSION=v0
IMAGE_NAME=$(IMAGE_ORG)/$(IMAGE_SHORT_NAME):$(MODEL_VERSION)
IMAGE_URL=$(REGISTRY_HOST):$(REGISTRY_PORT)/$(IMAGE_NAME)

watch_docker:
	@watch -n 1 'docker ps -a'

build_docker:
	docker build . -t $(IMAGE_URL)

build_kaniko_local:
# Requires local context to be authenticated to remote registry
	docker run -v $$PWD\:/workspace -v ~/.docker/config.json\:/kaniko/config.json --env DOCKER_CONFIG=/kaniko gcr.io/kaniko-project/executor\:latest -d $(IMAGE_NAME) \
	--build-arg url="www.seldon.io" --build-arg created="$$(date -u +'%Y-%m-%dT%H:%M:%SZ')" --build-arg version="$(MODEL_VERSION)" \
	--build-arg source="$$(git config --get remote.origin.url | rev | cut -f1 -d"@" | rev)" --build-arg revision="$$(git rev-parse HEAD)"

build_model_image:
	docker build . -t $(IMAGE_URL) \
	--build-arg url="www.seldon.io" --build-arg created="$$(date -u +'%Y-%m-%dT%H:%M:%SZ')" --build-arg version="$(MODEL_VERSION)" \
	--build-arg source="$$(git config --get remote.origin.url | rev | cut -f1 -d"@" | rev)" --build-arg revision="$$(git rev-parse HEAD)"

push_to_registry:
	docker push $(IMAGE_URL)

serve_local:
# Serving direct from python is on port 5000
	pip install -r requirements.txt
	exec seldon-core-microservice SimpleModel REST

start_serving_model:
	docker run --name=simple-model-service -d -p $(LOCAL_SERVICE_PORT):5000 $(IMAGE_URL)

stop_serving_model:
	docker rm -v -f simple-model-service

test_predict_call:
	curl -H "Content-Type: application/x-www-form-urlencoded" -d 'json={"data":{"names":["a"],"tensor":{"shape":[1,1],"values":[0]}}}' "http://127.0.0.1:$(LOCAL_SERVICE_PORT)/predict"

build_model_image_using_docker:
	docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock -v $$PWD:/work gsunner/s2i-tools:0.1 sh -c 'docker build . -t $(IMAGE_URL)'