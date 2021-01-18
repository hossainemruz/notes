---
description: Docker command cheat sheet.
---

# Commands

### Image

1. List all docker images

```bash
$ docker images
```

2. List only those images who has `none` substring in information

```bash
$ docker images | grep none
```

3. Print ID \(3rd field\) of the docker images who has `none` substring in their info.

```bash
$ docker images | grep none | awk '{print $3}'
```

4. Remove the images who has `none` substring in their information.

```bash
$ docker rmi -f (docker images | grep none | awk '{print $3}')
```

5. Free disk taken by docker images, volume etc.  


```bash
docker system prune --all
```

