# gstreamer-tutorial
 
## 1.- Contruir imagen

```
docker build -t gstreamer-image .
```

## 2.- Lanzar container

```
docker run -it --rm -v c:/sharedFolder:/shared gstreamer-image
```

## 3.- verificar version

```
gst-launch-1.0 --version
```

