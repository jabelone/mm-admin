FROM nginx:alpine
# based on this SO answer https://stackoverflow.com/questions/60233958

# Remove default NGINX Config
# Take care of Nginx logging
RUN rm /etc/nginx/conf.d/default.conf && \
    ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log

# NGINX Config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Resources
COPY html /var/www/html/

CMD ["nginx", "-g", "daemon off;"]