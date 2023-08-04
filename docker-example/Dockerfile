FROM python:3.11 as init
ARG API_URL
WORKDIR /app
COPY . .
ENV VIRTUAL_ENV=/app/venv HOME=/app
ENV PATH="$VIRTUAL_ENV/bin:$PATH"
RUN python3 -m venv $VIRTUAL_ENV && \
    pip install -r requirements.txt && \
    reflex init && \
    reflex export --frontend-only --no-zip


FROM python:3.11-slim
ARG API_URL
WORKDIR /app
RUN adduser --disabled-password --home /app reflex
COPY --chown=reflex --from=init /app /app
USER reflex
ENV PATH="/app/venv/bin:$PATH" API_URL=$API_URL

CMD reflex db migrate && reflex run --env prod

EXPOSE 3000
EXPOSE 8000
