# Remove Dangling Images

```
docker rmi $(docker images -f "dangling=true" -q)

docker volume rm $(docker volume ls -qf dangling=true)     # this freed up nearly 2GB!

docker rmi $(docker images | grep "^<none>" | awk '{print $3}')   # haven't tested this 
```
