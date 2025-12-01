# Use official nginx image
FROM nginx:alpine

# Copy custom nginx configuration file
COPY nginx.conf /etc/nginx/nginx.conf

# Copy custom configs
COPY conf.d/ /etc/nginx/conf.d/

# Copy static files to nginx default directory
COPY html/ /usr/share/nginx/html/

# Expose port 80
EXPOSE 80

# Start nginx server
# 可以不指定启动命令，nginx镜像默认会启动nginx服务
CMD ["nginx", "-g", "daemon off;"]
