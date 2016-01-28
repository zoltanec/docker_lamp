webraketa-docker-lamp
=====================
Docker lamp for development

Usage
-----

To create the image `webraketa/web-server`, execute the following command on the web-server folder:

	docker build -t webraketa/web-server .

Custom PHP application
-----------------------------------

After that, build the new project image in the web-project folder (replace sitename on your site name):

	docker build -t webraketa/sitename .

And run container:

	docker run -d -p 80:80 -v //c/local_site_path:/var/www/sitename webraketa/sitename

To get docker ip:

	docker-machine ls

Add in OS hosts file: docker_ip sitename