I have generated key pair and also a csr.

> akshay.csr  akshay.key

- Now create a kubernetes certificate-sigining-request with certificate-sigining-request.yaml

- Useful commands
    -   cat akshay.csr | base64 -w 0
    -   kubectl apply -f akshay-certificate-signing-request.yaml
    -   kubectl get csr
    -   kubectl certificate approve akshay
    -   kubectl get csr agent-smith -o yaml
    -   kubectl certificate deny agent-smith
    -   kubectl delete csr agent-smith
    -   