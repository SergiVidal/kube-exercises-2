# Questions

## 1. [Ingress Controller / Secrets] Crea los siguientes objetos de forma declarativa.

```console
kubectl create -f deploy.yaml
kubectl create -f svc.yaml
kubectl create -f ing.yaml
```

## 2. Modificar el archivo hosts para mapear la IP de minikube a la URL indicada en el Ingress.

* (NOTA: Mirar imagen "ex1.1.png")

```console
192.168.64.2 svidal.student.lasalle.com
```

* (Validación: Mirar imagen "ex1.2.png")

svidal.student.lasalle.com

## 3. Generar el certificado.

```console
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls-key.key -out tls-cert.crt  
```

* (NOTA: Mirar imagen "ex1.3.png")

## 4. Crear un secret que contenga el certificado.

```console
kubectl create secret tls tls-certificate --key tls-key.key --cert tls-cert.crt   
```

## 5. Crear el nuevo ingres cogiendo como base el ingres "ing.yaml" pero añadiendole la configuracion anterior.

```console
kubectl create -f ing2.yaml
```

* (Validación: Mirar imagen "ex1.4.png")

svidal.student.lasalle.com
