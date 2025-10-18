# Pooja Invitation Docker

A containerized web invitation for your Pooja ceremony.

## Build and Run

```bash
# Build the Docker image
docker build -t pooja-invitation .

# Run the container
docker run -p 8080:80 pooja-invitation
```

Then open http://localhost:8080 in your browser.

## Deploy Options

**Local sharing on network:**
```bash
# Run on all network interfaces
docker run -p 80:80 pooja-invitation
```
Then share your local IP (e.g., http://192.168.1.100) with guests.

**Cloud deployment:**
- Push to Docker Hub and deploy on any cloud platform
- Use with cloud services like AWS ECS, Google Cloud Run, or Azure Container Instances

## Files Structure
- `index.html` - Main invitation page
- `style.css` - Styling
- `Dockerfile` - Container configuration
