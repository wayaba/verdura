# verdura
cosas para Ionic

ibmcloud cr token-add --description "la descripcion" --non-expiring -q

kubectl --namespace api-afip create secret docker-registry soa-secret --docker-server=registry.ng.bluemix.net --docker-username=token --docker-password=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI1OGI0MWE2ZS00ZjViLTU1MzgtYjZlNy1mNGFlNjMzNmIwZTciLCJpc3MiOiJyZWdpc3RyeS5uZy5ibHVlbWl4Lm5ldCJ9.6siYFXm5Ss6XvKfVJ4144WAMk7JIWxCKcHS9H7I6Xwc --docker-email=ppedraza.sofrecom@supervielle.com.ar


kubectl --namespace api-afip create secret docker-registry soa-secret --docker-server=registry.ng.bluemix.net --docker-username=token --docker-password=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI1OGI0MWE2ZS00ZjViLTU1MzgtYjZlNy1mNGFlNjMzNmIwZTciLCJpc3MiOiJyZWdpc3RyeS5uZy5ibHVlbWl4Lm5ldCJ9.6siYFXm5Ss6XvKfVJ4144WAMk7JIWxCKcHS9H7I6Xwc --docker-email=ppedraza.sofrecom@supervielle.com.ar


eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2Mzg5ODkxZC00YjllLTUxMmUtYTFiNi0yYmFiNjZhZDczMTkiLCJpc3MiOiJyZWdpc3RyeS5uZy5ibHVlbWl4Lm5ldCJ9.CA4FishqKQWt9e-QkerO1yU9OeuL_-CZom6Ba7YaSWo


# Obtener los pods de tu namespace
kubectl --namespace={nombre del namespace} get pods
Ejemplo:
kubectl --namespace=api-afip get pods

# Entrar al shell dentro de un container de tu namespace
kubectl --namespace={nombre del namespace} exec -it {nombre del pod} --container {nombre del container} -- /bin/bash
Ejemplo:
kubectl --namespace=api-afip exec -it api-afip-5d947b5b77-7ww8v --container api-afip -- /bin/bash
