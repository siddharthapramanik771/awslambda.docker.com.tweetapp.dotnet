FROM public.ecr.aws/lambda/dotnet:6 AS base

FROM mcr.microsoft.com/dotnet/sdk:6.0-bullseye-slim as build
WORKDIR /src
COPY ["com.tweetapp.csproj", "com.tweetapp/"]
RUN dotnet restore "com.tweetapp/com.tweetapp.csproj"

WORKDIR "/src/com.tweetapp"
COPY . .
RUN dotnet build "com.tweetapp.csproj" --configuration Release --output /app/build

FROM build AS publish
RUN dotnet publish "com.tweetapp.csproj" \
            --configuration Release \ 
            --runtime linux-x64 \
            --self-contained false \ 
            --output /app/publish \
            -p:PublishReadyToRun=true  

FROM base AS final
WORKDIR /var/task

CMD ["com.tweetapp::com.tweetapp.LambdaEntryPoint::FunctionHandlerAsync"]
COPY --from=publish /app/publish .