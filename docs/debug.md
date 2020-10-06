# Troubleshooting
The following is a (growing) list of possible errors you might run in to when setting up the CSI Driver provider for auzre, along with recommendations for remediation.

## Container deployment failing with "failed to find provider binary azure"
### Symptom
When attempting to install your service that leverages the newly created SecretProvider, your container deployment might not succeed. If you run `kubectl describe pod my_pod` you might see something like this:
```
Events:
  Type     Reason       Age                            From                                        Message
  ----     ------       ----                           ----                                        -------
  Normal   Scheduled    <invalid>                      default-scheduler                           Successfully assigned default/nginx-secrets-store-inline to aks-agentpool-36317948-vmss000001
  Warning  FailedMount  <invalid> (x3 over <invalid>)  kubelet, aks-agentpool-36317948-vmss000001  MountVolume.SetUp failed for volume "secrets-store-inline" : rpc error: code = Unknown desc = failed to mount secrets store objects for pod default/nginx-secrets-store-inline, err: failed to find provider binary azure, err: stat /etc/kubernetes/secrets-store-csi-providers/azure/provider-azure: no such file or directory
```

Particularly this part: **_err: failed to find provider binary azure_**
### Explanation
This means the keyvault provider was not installed as part of the CSI driver installation.
### Solution
If the CSI driver was installed via helm, uninstall it and reinstall from the correct package found [here](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/charts/csi-secrets-store-provider-azure/README.md)

If installed via deployment files, follow the instructions [here](https://github.com/Azure/secrets-store-csi-driver-provider-azure/blob/master/docs/install-yamls.md) to install the keyvault provider
