# --- STAGE 1: Runtime (Nginx Alpine) ---
FROM nginx:1.25.4-alpine

# Create non-root user (nginx image usually has nginx user, but let's ensure proper permissions)
WORKDIR /usr/share/nginx/html

# Copy static frontend files
COPY index.html .

# Expose port
EXPOSE 80

# Health check
HEALTHCHECK --interval=30s --timeout=3s CMD wget --no-verbose --tries=1 http://localhost/ || exit 1

CMD ["nginx", "-g", "daemon off;"]
