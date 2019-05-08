FROM python:3.7-slim
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
EXPOSE 5000

# Define environment variable
ENV MODEL_NAME SimpleModel
ENV API_TYPE REST
ENV SERVICE_TYPE MODEL
ENV PERSISTENCE 0

CMD exec seldon-core-microservice $MODEL_NAME $API_TYPE --service-type $SERVICE_TYPE --persistence $PERSISTENCE

ARG title=unknown
ARG description=unknown
ARG url=unknown
ARG source=unknown
ARG vendor=unknown
ARG revision=unknown
ARG created=unknown
ARG version=unknown

LABEL org.opencontainers.image.title="${title}" \
      org.opencontainers.image.description="${description}" \
      org.opencontainers.image.url="${url}" \
      org.opencontainers.image.source="${source}" \
      org.opencontainers.image.created="${created}" \
      org.opencontainers.image.version="${version}" \
      org.opencontainers.image.vendor="${vendor}" \
      org.opencontainers.image.revision="${revision}"