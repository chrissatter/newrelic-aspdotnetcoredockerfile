# This Dockerfile will construct a Docker container with the New Relic ASP.NET Core APM agent
#
# Created by ACS of New Relic.
# 11/10/2019 - Initial Release
#

# grab dotnet core image from Docker Repo
FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
WORKDIR /app
# copy csproj and restore as distinct layers
COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
RUN dotnet restore
# copy everything else and build app
COPY aspnetapp/. ./aspnetapp/

WORKDIR /app/aspnetapp
RUN dotnet publish -c Release -o out
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS runtime

# Install the agent
RUN apt-get update && apt-get install -y wget ca-certificates gnupg \
&& echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list \
&& wget https://download.newrelic.com/548C16BF.gpg \
&& apt-key add 548C16BF.gpg \
&& apt-get update \
&& apt-get install -y newrelic-netcore20-agent

# Enable the agent
ENV CORECLR_ENABLE_PROFILING=1 \
CORECLR_PROFILER={36032161-FFC0-4B61-B559-F6C5D41BAE5A} \
CORECLR_NEWRELIC_HOME=/usr/local/newrelic-netcore20-agent \
CORECLR_PROFILER_PATH=/usr/local/newrelic-netcore20-agent/libNewRelicProfiler.so \
NEW_RELIC_LICENSE_KEY=YOUR_LICENSE_KEY \
NEW_RELIC_APP_NAME=HelloWorld-ASPDOTNET \
ENV NEW_RELIC_DISTRIBUTED_TRACING_ENABLED=true
WORKDIR /app
COPY --from=build /app/aspnetapp/out .
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
