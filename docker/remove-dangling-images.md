# Remove Dangling Images

```
docker rmi $(docker images -f "dangling=true" -q)
```