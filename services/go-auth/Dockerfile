# We establish a separate stage for building the app.
# Next, we embrace the powerful and versatile Debian base image 🐳
# This choice grants us easier access to essential build utilities and a robust development environment.
FROM golang:1.21-bullseye AS build

WORKDIR /app 

# We optimize our path to discovery, selecting only the files required to install dependencies. 🧭
# With this choice, we unlock the potential of better layer caching, improving our image's efficiency.
COPY go.mod go.sum ./

RUN go mod download

COPY . .

# During this stage, we compile our application ahead of time, avoiding any runtime surprises.
# The resulting binary, web-app-golang, will be our steadfast companion in the final leg of our journey.
# We strategically add flags to statically link our binary.
RUN go build \
  -ldflags="-linkmode external -extldflags -static" \
  -tags netgo \
  -o web-app-golang

# The scratch base image welcomes us as a blank canvas for our prod stage.
FROM scratch

WORKDIR /

# We transport the binary to our deployable image
COPY --from=build /app/web-app-golang web-app-golang

CMD ["/web-app-golang"]