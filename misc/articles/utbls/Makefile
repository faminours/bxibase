ARTICLE_NAME=utbls

all: run

dev: 
	echo "Running in development mode -- You cann access it through http://localhost:8080"
	echo "All modifications made in the notebook will impacted the sources in the resources folder "
	docker rm -f $(ARTICLE_NAME).dev || /bin/true
	docker run  --name $(ARTICLE_NAME).dev --publish 8080:8080 --detach --volume ${PWD}/resources:/srv openbxi/centos-bxibase-jupyter

image:
	echo "Making the docker image for final publication on dockerhub"
	docker build --tag openbxi/$(ARTICLE_NAME) \
		     --rm=true \
		     --file Dockerfile.$(ARTICLE_NAME) \
		     --build-arg HTTP_PROXY=$(http_proxy)\
		     --build-arg HTTPS_PROXY=${https_proxy}\
		     --build-arg FTP_PROXY=${ftp_proxy}\
		     --build-arg NO_PROXY=${no_proxy}\
		    ${PWD}

run: image
	echo "Running in production mode  -- You can access it through http://localhost:8080"
	echo "All modifications made in the notebook will *not* be impacted anywhere!"
	docker rm -f $(ARTICLE_NAME) || /bin/true
	docker run  --name $(ARTICLE_NAME) --publish 8080:8080 --detach openbxi/$(ARTICLE_NAME)
