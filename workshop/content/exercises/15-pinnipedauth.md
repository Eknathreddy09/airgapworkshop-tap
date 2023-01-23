
Pinniped is used to support authentication on Tanzu Application Platform. This topic introduces how to install Pinniped on a single cluster of Tanzu Application Platform. You will deploy two Pinniped components into the cluster.

The Pinniped Supervisor is an OIDC server which allows users to authenticate with an external identity provider (IDP). It hosts an API for the concierge component to fulfill authentication requests.

The Pinniped Concierge is a credential exchange API that takes a credential from an identity source, for example, Pinniped Supervisor, proprietary IDP, as input. The Pinniped Concierge authenticates the user by using the credential, and returns another credential that is parsable by the host Kubernetes cluster or by an impersonation proxy that acts on behalf of the user.

```dashboard:open-url
url: https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.3/tap/GUID-authn-authz-overview.html
```
