image_built:
	docker build -t quartus:15.1 .
	touch image_built

clean:
	rm -f image_built
	docker image rm quartus:15.1
