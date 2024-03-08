DOCKER ?= docker
TARGET ?= fop2/server:2.31.39
TARGET_LATEST ?= fop2/server:latest

all:
    @echo "Building ${TARGET}"
    $(DOCKER) build -t $(TARGET) -f Dockerfile .

push:
    @echo "Push ${TARGET} and $(TARGET_LATEST)"
    $(DOCKER) push $(TARGET)
    $(DOCKER) tag $(TARGET) $(TARGET_LATEST)
    $(DOCKER) push $(TARGET_LATEST)
