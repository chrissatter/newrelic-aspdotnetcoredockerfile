Please make sure to update the lines in the Docker file:

NEW_RELIC_LICENSE_KEY=YOUR_LICENSE_KEY \
NEW_RELIC_APP_NAME=HelloWorld-ASPDOTNET \

Then to run the container:

docker build -t aspnetapp .
docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp

point browser to http://localhost:5000
