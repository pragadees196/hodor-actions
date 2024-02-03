# Use an official Go runtime as a parent image
FROM golang:alpine

# Set the working directory to the app directory inside the container
WORKDIR /app

# Copy the local code to the container
COPY . .

# Install Git to fetch dependencies (if needed)
RUN apk add --no-cache git

# Build the Go application
RUN go build -o main .

# Expose the port the app runs on
EXPOSE 8081

# Run the binary program produced by `go build`
CMD ["./main"]

