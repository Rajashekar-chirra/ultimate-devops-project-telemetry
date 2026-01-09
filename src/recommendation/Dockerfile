FROM python:3.10-slim-bookworm AS builder
WORKDIR /usr/src/app/
COPY requirements.txt ./
RUN pip install --upgrade pip
RUN pip install -r requirements.txt
COPY . . 
FROM python:3.10-alpine AS release
WORKDIR /usr/src/app/
COPY --from=builder /usr/src/app/ . 
ENTRYPOINT ["python3","recommendation_server.py"] 
