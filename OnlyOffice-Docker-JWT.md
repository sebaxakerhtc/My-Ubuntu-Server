Sooooo....
Every restart of server/Docker container it gives me a new JWT_secret.
I DON'T NEED IT!

After docker inspec we have start script `/app/ds/run-document-server.sh`
So, let's stop this stupid action of generation secret with every start/restart

We need to edit this script
```
docker exec -it [container] nano /app/ds/run-document-server.sh
```

Find
```
JWT_SECRET=${JWT_SECRET:-$(pwgen -s 20)}
```
and replace it with
```
JWT_SECRET=${JWT_SECRET:-$(echo MyStrongSecret)}
```
where MyStrongSecret - is your own secret

Restart the services
```
docker exec [container] supervisorctl restart all
```

# Enjoy!
