FROM nginx:1.14-alpine


RUN adduser -S -D -H -u 1001 -h /app -s /sbin/nologin -g app app  
RUN mkdir -p /app
RUN rm -r /etc/nginx/nginx.conf
 
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./web.conf /etc/nginx/conf.d/default.conf
COPY ./homework.html /app/homework.html
RUN touch /var/run/nginx.pid && \
  chown -R app: /var/run/nginx.pid && \
  chown -R app: /var/cache/nginx && \
  chown -R app: /etc/nginx && \
  chown -R app: /app

EXPOSE 8000
STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]
USER app