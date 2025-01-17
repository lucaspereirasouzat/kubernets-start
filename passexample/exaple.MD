Create a Secret from a Definition Manifest

We can create a Secret manually from a YAML definition manifest. The example manifest below is named mypass.yaml. There are two types of maps for sensitive information inside a Secret: data and stringData.

With data maps, each value of a sensitive information field must be encoded using base64. If we want to have a definition manifest for our Secret, we must first create the base64 encoding of our password:

$ echo mysqlpassword | base64

bXlzcWxwYXNzd29yZAo=

and then use it in the definition manifest:

apiVersion: v1
kind: Secret
metadata:
  name: my-password
type: Opaque
data:
  password: bXlzcWxwYXNzd29yZAo=

Please note that base64 encoding does not mean encryption, and anyone can easily decode our encoded data:

$ echo "bXlzcWxwYXNzd29yZAo=" | base64 --decode

mysqlpassword

Therefore, make sure you do not commit a Secret's definition file in the source code.

With stringData maps, there is no need to encode the value of each sensitive information field. The value of the sensitive field will be encoded when the my-password Secret is created:

apiVersion: v1
kind: Secret
metadata:
  name: my-password
type: Opaque
stringData:
  password: mysqlpassword

Using the mypass.yaml definition file we can now create a secret with kubectl create command: 

$ kubectl create -f mypass.yaml

secret/my-password created
