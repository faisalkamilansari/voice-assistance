# ------------ Stage 1: Build ------------
    FROM node:18 AS builder

    WORKDIR /app
    
    # Install pnpm
    RUN npm install -g pnpm
    
    # Copy only necessary files for installing dependencies
    COPY package.json pnpm-lock.yaml ./
    
    # Install dependencies (with cache layer optimization)
    RUN pnpm install --frozen-lockfile
    
    # Copy rest of the source code
    COPY . .
    
    # Build the app
    RUN pnpm run build
    
    
    # ------------ Stage 2: Runtime ------------
    FROM node:18-slim
    
    WORKDIR /app
    
    # Install pnpm
    RUN npm install -g pnpm && \
        apt-get purge -y --auto-remove && \
        rm -rf /var/lib/apt/lists/* /tmp/* /root/.npm /root/.cache
    
    # Copy only necessary files for runtime
    COPY --from=builder /app/.next ./.next
    COPY --from=builder /app/node_modules ./node_modules
    COPY --from=builder /app/package.json ./package.json

    
    EXPOSE 3000
    
    CMD ["pnpm", "start"]
    