# cloud-native-handson

## Install Kubectl
https://kubernetes.io/docs/tasks/tools/install-kubectl/



## Download the Kubeconfig

Install unter `~/.kube/config`

````yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURDekNDQWZPZ0F3SUJBZ0lRRTJVTnk0S3o3RmFsbWtMSVIyd2svekFOQmdrcWhraUc5dzBCQVFzRkFEQXYKTVMwd0t3WURWUVFERXlReE9EZGxaR0l6TUMxaE9ESXhMVFF6TnpBdFltVTFaQzA0WldVMVlqVTFNemM1TldJdwpIaGNOTVRreE1qRTBNRFkwTURNMFdoY05NalF4TWpFeU1EYzBNRE0wV2pBdk1TMHdLd1lEVlFRREV5UXhPRGRsClpHSXpNQzFoT0RJeExUUXpOekF0WW1VMVpDMDRaV1UxWWpVMU16YzVOV0l3Z2dFaU1BMEdDU3FHU0liM0RRRUIKQVFVQUE0SUJEd0F3Z2dFS0FvSUJBUUNreFlzSTk2WmFScjZaRzR1a3JRcGFTMkw3eHZEeHJ6WTRpTFMrRGdYNQo0R3ltUi9ZVUdKQyt5SGNwNk9PRnJJQzhJTmpXSnZrQW9zZFZNb09FQkdmb3NKVnFpcjJ6OVN1Uk52OWFzSUxHCnlVbkRSRVJpbXhlSWRtL0dDWVFLUWJYR2diT1hsZmtPY2pRNm1LdE93QVo2SU5Rd1FGV29jbTViNTZlYmRmVjkKQk5tblV5a2FJMmlyTVAyalRVRmtVS3lOYnpxVGVwMUpJdUhrNXNKOElJT2VGSmd1ZjhBOW1sT1RYZU02eHZvYwphczdTekpVNmJGSGpBN0NqTFhZM1BhYlkxRGhyWklyTllPVlVWTHNnbWtweWIrUzJZRE1IMzA1czFOYzRWaHFvCkFvSmtTNE5GdHcwNlB1ZGh3M25BNGhGRlVqK0xOY3dxNkZqQW81MnlaMTJQQWdNQkFBR2pJekFoTUE0R0ExVWQKRHdFQi93UUVBd0lDQkRBUEJnTlZIUk1CQWY4RUJUQURBUUgvTUEwR0NTcUdTSWIzRFFFQkN3VUFBNElCQVFCZwpoUjBUd2h1M0greG1wUlM2aGpFdWJFbGg4TzE0QWR0dENSK3VWVDhDSStyN2plK1lGdUVyNlBYNTZKeEhyd1B6Cm1RZmttSnlOQU8wSDhIeGYwQ1QzSWhEQzFQNjVOd2M2WWFzTFlhWDJuWkpNYWw1a0IvbnhxcXNLV0VnSzV0bWMKRWc3Z0M1Rll1YkdHbkx4QjMxOWx3OTRXUVg0SlZ2NWlKMXZ4N294RmpObm93ZE5KQ2NUNkYvMWtRNlI4emt5VwpTOU9DWUNkaDdyWWVrQUJrTTlPbSsreU9QbU9PUmRwYTZlOFV6ZG1ENExIS2FiZmhqSGNIRWlvRjd5WXVBc0FvClZ3U0NtY1ZpdndaOVhHcWJtTkovbjA1YTIrWlcvbERObitMNmhNUTRWcWVJcXo0OUh2RnhnK1EySk5QOEZGOWsKYmMweE8za28xS3RKZ1Bmby81QU4KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
    server: https://35.205.254.27
  name: gke_hypnotic-surge-215419_europe-west1-b_cn-workshop
contexts:
- context:
    cluster: gke_hypnotic-surge-215419_europe-west1-b_cn-workshop
    user: gke_hypnotic-surge-215419_europe-west1-b_cn-workshop
  name: gke_hypnotic-surge-215419_europe-west1-b_cn-workshop
current-context: gke_hypnotic-surge-215419_europe-west1-b_cn-workshop
kind: Config
preferences: {}
users:
- name: gke_hypnotic-surge-215419_europe-west1-b_cn-workshop
  user:
    auth-provider:
      config:
        access-token: ya29.Ima1Bzi5SjMVuDHI1XAetQxD15pGyvFMg6b53ysgToIqFdbbaJPNmzMHM0XfRSw2bPs_3PVTqFX0LZE777CFp78KVqsOrHn6_5Y4dC53Yx2Vj0_iP29JJTNW_aSsLI2xhKlA5koVZho
        cmd-args: config config-helper --format=json
        cmd-path: /Users/michaelh/google-cloud-sdk/bin/gcloud
        expiry: "2019-12-17T10:26:43Z"
        expiry-key: '{.credential.token_expiry}'
        token-key: '{.credential.access_token}'
      name: gcp
````