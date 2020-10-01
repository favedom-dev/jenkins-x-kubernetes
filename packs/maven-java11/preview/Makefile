OS := $(shell uname)
TLS_SECRET := tls-favedom-dev-softcannery-com-p
PREVIEW_NAMESPACE := $(shell tr '[:upper:]' '[:lower:]' <<< $(APP_NAME)-$(BRANCH_NAME))

preview: namespace tls-secret
ifeq ($(OS),Darwin)
	sed -i "" -e "s/version:.*/version: $(PREVIEW_VERSION)/" Chart.yaml
	sed -i "" -e "s/version:.*/version: $(PREVIEW_VERSION)/" ../*/Chart.yaml
	sed -i "" -e "s/tag:.*/tag: $(PREVIEW_VERSION)/" values.yaml
else ifeq ($(OS),Linux)
	sed -i -e "s/version:.*/version: $(PREVIEW_VERSION)/" Chart.yaml
	sed -i -e "s/version:.*/version: $(PREVIEW_VERSION)/" ../*/Chart.yaml
	sed -i -e "s|repository:.*|repository: $(DOCKER_REGISTRY)\/REPLACE_ME_DOCKER_REGISTRY_ORG\/REPLACE_ME_APP_NAME|" values.yaml
	sed -i -e "s/tag:.*/tag: $(PREVIEW_VERSION)/" values.yaml
else
	echo "platfrom $(OS) not supported to release from"
	exit -1
endif
	echo "  version: $(PREVIEW_VERSION)" >> requirements.yaml
	sed -i -e "s|tlsSecretName:.*|tlsSecretName: $(TLS_SECRET)|" values.yaml
	
	jx step helm build

namespace:
	kubectl create ns $(PREVIEW_NAMESPACE) || true

tls-secret:	
	kubectl get secret $(TLS_SECRET) --namespace=jx --export -o yaml | kubectl apply --namespace=$(PREVIEW_NAMESPACE) -f -	
