FROM python:3.15-rc-alpine3.23
WORKDIR /app
RUN pip install flask

ENV APP_MODE=production
ENV APP_REGION=canada-west 

COPY app.py .
CMD ["python","app.py"]
