# docker_lamp
dockerfiles lamp for development

Install example

cd web-server

docker build -t webraketa/web-server .

docker run -p 80:80 -d -v //c/local_site_path:/var/www/site webraketa/web-server



