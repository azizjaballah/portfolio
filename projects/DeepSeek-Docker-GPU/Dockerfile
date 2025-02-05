FROM ollama/ollama:latest

# Set working directory
WORKDIR /app

# Expose necessary ports
EXPOSE 11434

# Pull the DeepSeek-R1:33B model
RUN ollama pull deepseek-r1:33b

# Start the server
CMD ["ollama", "serve"]
