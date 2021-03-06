---

copyright:
  years: 2020
lastupdated: "2020-09-14"

keywords: code engine, registry, container registry, image registry

subcollection: codeengine

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Adding access to a private container registry 
{: #add-registry}

A container registry contains images that you can create with a build and use in your application or job. You must first add access to a container registry before you can push or pull the images.
{: shortdesc}

To plan your options for images, see [planning image registries](/docs/codeengine?topic=codeengine-plan-image).

To add access to a Docker Hub account, you need your account name and password (or [access token](#add-registry-access-docker)). To add access to a {{site.data.keyword.registryshort}} instance, you need an IAM API key.

## Create an IAM API key for a {{site.data.keyword.registryshort}} instance that is in your account
{: #access-registry-account}

If you are accessing a {{site.data.keyword.registryshort}} instance that is in the same account as your {{site.data.keyword.codeengineshort}} instance, you must first create an IAM API key and then provide the key to {{site.data.keyword.codeengineshort}}.

### Creating an API key from the console
{: #access-registry-account-console}

To create an {{site.data.keyword.cloud_notm}} IAM API key from the console,

1. Launch [Access (IAM) Overview](https://cloud.ibm.com/iam/overview).
2. Select **API keys**.
3. Click **Create API key**.
4. Enter a name and optional description for your API key and click **Create**.
5. Copy the API key or click download to save it. 

   You won’t be able to see this API key again, so be sure to record it in a safe place.
   {: important}
   
Now that you created your API key, continue to [Adding {{site.data.keyword.registryshort}} access to {{site.data.keyword.codeengineshort}}](#add-registry-access-ce).
   
### Creating an API key with the CLI
{: #access-registry-account-cli}

To create an {{site.data.keyword.cloud_notm}} IAM API key from the CLI, run the [`iam api-key-create`](/docs/account?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_api_key_create) command. For example, to create an API key called `cliapikey` with a description of "My CLI APIkey" and save it to a file called `key_file`, run the following command:

```
ibmcloud iam api-key-create cliapikey -d "My CLI APIkey" --file key_file
```
{: pre}

If you choose to not save your key to a file, you must record the apikey that is displayed when you create it. You cannot retrieve it later.
{: important}

Now that you created your API key, continue to [Adding {{site.data.keyword.registryshort}} access to {{site.data.keyword.codeengineshort}}](#add-registry-access-ce).

## Create an access token for Docker Hub
{: #add-registry-access-docker}

In order to create access to a Docker Hub account, you must provide your password or an access token. By using an access token, you can more easily grant and revoke access to your Docker Hub account without requiring a password change. For more information about access tokens and Docker Hub, see [Managing access tokens](https://docs.docker.com/docker-hub/access-tokens/){: external}.
   
## Add registry access to {{site.data.keyword.codeengineshort}}
{: #add-registry-access-ce}

After you create your IAM API key, you can authorize {{site.data.keyword.codeengineshort}} to pull images from your container registry.
   
### Adding registry access from the console
{: #add-registry-access-ce-console}

To add {{site.data.keyword.registryshort}} or Docker Hub access with the console:

1. Go to the [{{site.data.keyword.codeengineshort}} dashboard](https://cloud.ibm.com/codeengine/overview).
2. Select a project (or [create one](/docs/codeengine?topic=codeengine-manage-project#create-a-project)).
3. From the project page, click **Registry access**.
4. Click **Add registry access**.
5. Enter a name for your registry access.
6. Enter a server name for your registry access. For {{site.data.keyword.registryshort}}, the server name is `<region>.icr.io`. For example, `us.icr.io`. For [Docker Hub](https://hub.docker.com/), the server name is `https://index.docker.io/v1/`.
7. Enter the username. For {{site.data.keyword.registryshort}}, it is `iamapikey`. For Docker Hub, it is your Docker ID.
8. Enter the password. For {{site.data.keyword.registryshort}}, this is your API key. For Docker Hub, you can use your Docker Hub password or an [access token](#add-registry-access-docker).
9. Click **Done**.

You can add access to a container registry when you create an application or job, or when you build an image. Click **Select image** and then **Add registry**. Follow previous steps 5-9.
{: tip}

## Adding registry access with the CLI
{: #add-registry-access-ce-cli}

To add {{site.data.keyword.registryshort}} or Docker Hub access with the CLI, use the `registry create` command.

```
ibmcloud ce registry create --name  --server SERVER --username USER_NAME --password API_KEY
```
{: pre}
<table>
  <caption><code>registry create</code> command components</caption>
   <thead>
    <col width="25%">
    <col width="75%">
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>registry create</code></td>
   <td>The command to create your registry access.</td>
   </tr>
   <tr>
   <td><code>--name</code></td>
   <td>The name of the image registry access secret. Use a name that is unique within the project. This value is required.
     <ul>
     <li>The name must begin and end with a lowercase alphanumeric character.</li>
	   <li>The name must be 253 characters or fewer and can contain lowercase letters, numbers, periods (.), and hyphens (-).</li>
     </ul>
   </td>
   </tr>
   <tr>
   <td><code>--server</code></td>
   <td>Enter the server name for the registry server. For {{site.data.keyword.registryshort}}, the server name is `<region>.icr.io`. For example, `us.icr.io`. For [Docker Hub](https://hub.docker.com/), the value is `https://index.docker.io/v1/`.</td>
   </tr>
   <tr>
   <td><code>--username</code></td>
   <td>Enter the username. For {{site.data.keyword.registryshort}}, it is `iamapikey`. For Docker Hub, it is your Docker ID.</td>
   </tr>
   <tr>
   <td><code>--password</code></td>
   <td>Enter the password. For {{site.data.keyword.registryshort}}, this is your API key. For Docker Hub, you can use your Docker Hub password or an [access token(#add-registry-access-docker).</td>
   </tr>
   </tbody></table>

For example, create registry access to a {{site.data.keyword.registryshort}} instance called `myregistry` that is located at `us.icr.io`:

```
ibmcloud ce registry create --name myregistry --server us.icr.io --username iamapikey --password API_KEY
```
{: pre}

## Setting up access for a {{site.data.keyword.registryshort}} instance from a different account
{: #access-registry-diff-account}

You can assign {{site.data.keyword.cloud_notm}} IAM access policies to users or a service ID to restrict permissions to specific registry image namespaces or actions (such as push or pull). Then, create an API key and store these registry credentials in {{site.data.keyword.codeengineshort}}.
{: shortdesc}

For example, to access images in other {{site.data.keyword.cloud_notm}} accounts, create an API key that stores the {{site.data.keyword.registryfull_notm}} credentials of a user or service ID in that account. Then, in {{site.data.keyword.codeengineshort}}, save the API key credentials so that you can pull the images to create applications and jobs.

The following steps create an API key that stores the credentials of an {{site.data.keyword.cloud_notm}} IAM service ID. Instead of using a service ID, you might want to create an API key for a user ID that has an {{site.data.keyword.cloud_notm}} IAM service access policy to {{site.data.keyword.registryfull_notm}}. However, make sure that the user is a functional ID or have a plan in place in case the user leaves so that {{site.data.keyword.codeengineshort}} can still access the registry.
{: note}

**Before you begin**

- [Set up a namespace in {{site.data.keyword.registryfull_notm}} and push images to this namespace](/docs/Registry?topic=Registry-getting-started#gs_registry_namespace_add).
- Log in to your account.

### Authorizing access to {{site.data.keyword.registryshort}} from console
{: #authorize-cr-console}

In order to pull or push images from or to {{site.data.keyword.registryfull_notm}}, you must create a service ID, create an access policy for the service ID, and then create an API key to store the credentials.

#### Step 1: Create the service ID and authorize it to the Container Registry service

1. Launch [Access (IAM) Overview](https://cloud.ibm.com/iam/overview).
2. Select **Service IDs**.
3. If you have a Service ID that you want to use, select it. If not, select **Create**, enter a name and description,  and click **Create**.
4. From the Service ID page, select **Access policies** and then **Assign access**.
5. From the **Assign service ID additional access** section,
    1. Select **IAM services**.
    2. Select **Container Registry** for type of access.
    3. Select the type of access: **Account**, **All resource groups**, or a specific resource group.
    4. Optionally, select a **Region**, **Resource type**, or **Resource name** to further restrict access.
    5. For Service access, select the type of access you want to grant. If you plan to only use images for your applications and jobs, select **Reader**. If you want to push the outcome of builds, then also select **Writer**.
    6. Click **Add** and then **Assign**.

#### Step 2: Authorize service ID to get details of an API key with the console
{: #authorize-service-id}

Now that our service ID is created and is granting access to {{site.data.keyword.registryshort}}, you must authorize the service ID to also get details of an API key by giving the service ID IAM Identity service access for Account management.

1. From the Service ID page, select **Access policies** and then **Assign access**.
2. From the **Assign service ID additional access** section,
    1. Select **Account management**.
    2. Select **IAM Identity service** for type of access to assign.
    3. Optionally, select a **Region**, **Resource type**, or **Resource name** to further restrict access.
    4. For Platform access, select **Operator** access or higher.
    5. Click **Add** and then **Assign**.
    
#### Step 3: Creating an API key for a service ID
{: #create-api-key}

Create an API key for a service ID.

1. From the Service ID page, select **API keys** and then **Create**.
2. Enter a name and optional description for your API key and click **Create**.
3. Copy the API key or click download to save it. 

   You won’t be able to see this API key again, so be sure to record it in a safe place.
   {: important}

Now that you have your access policies in place for your service ID and your API key created, you can [add access to {{site.data.keyword.codeengineshort}}](#add-registry-access-ce) in order to pull images from your container registry.

### Authorizing access to {{site.data.keyword.registryshort}} with the CLI
{: #authorize-cr-cli}

In order to pull images from {{site.data.keyword.registryfull_notm}}, you must create a service ID, create access policies for the service ID, and then create an API key to store your credentials.

1.  Create an {{site.data.keyword.cloud_notm}} IAM service ID for your cluster that is used for the IAM policies and API key credentials in the image pull secret. Be sure to give the service ID a description that helps you retrieve the service ID later, such as including both the cluster and namespace name.

    ```
    ibmcloud iam service-id-create codeengine-<project_name>-id --description "Service ID for IBM Cloud Container Registry in Code Engine project <project_name>"
    ```
    {: pre}
    
    For example, create a service ID called `codeengine-myproj-id` with the description `Service ID for IBM Cloud Container Registry in Code Engine project myproj`:
    
    ```
    ibmcloud iam service-id-create codeengine-myproj-id --description "Service ID for IBM Cloud Container Registry in Code Engine project my proj"
    ```
    {: pre}
    
2.  Create a custom {{site.data.keyword.cloud_notm}} IAM policy for your service ID that grants access to {{site.data.keyword.registrylong_notm}}.

    ```
    ibmcloud iam service-policy-create <service_ID> --roles <service_access_role> --service-name container-registry [--region <IAM_region>] [--resource-type namespace --resource <registry_namespace>]
    ```
    {: pre}
    <table summary="The columns are read from left to right. The first column has the parameter of the command. The second column describes the parameter.">
    <caption>Understanding this command's components</caption>
    <col width="25%">
    <thead>
    <th>Component</th>
    <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;service_ID&gt;</em></code></td>
    <td>Required. Replace with the `codeengine-<project_name>-id` service ID that you previously created in step 1.</td>
    </tr>
    <tr>
    <td><code>--service-name <em>container-registry</em></code></td>
    <td>Required. Enter `container-registry` to create an IAM policy for {{site.data.keyword.registrylong_notm}}.</td>
    </tr>
    <tr>
    <td><code>--roles <em>&lt;service_access_role&gt;</em></code></td>
    <td>Required. Enter the [service access role for {{site.data.keyword.registrylong_notm}}](/docs/Registry?topic=Registry-iam#service_access_roles) that you want to scope the service ID access to. Possible values are `Reader`, `Writer`, and `Manager`. If you are pulling images, then `Reader` is sufficient.</td>
    </tr>
    <tr>
    <td><code>--region <em>&lt;IAM_region&gt;</em></code></td>
    <td>Optional. If you want to scope the access policy to certain IAM regions, enter the regions in a comma-separated list. Possible values are `global` and the [local registry regions](/docs/Registry?topic=Registry-registry_overview#registry_regions_local).</td>
    </tr>
    <tr>
    <td><code>--resource-type <em>namespace</em> --resource <em>&lt;registry_namespace&gt;</em></code></td>
    <td>Optional. If you want to limit access to only images in certain [{{site.data.keyword.registrylong_notm}} namespaces](/docs/Registry?topic=Registry-registry_setup_cli_namespace#registry_setup_cli_namespace_plan), enter `namespace` for the resource type and specify the `<registry_namespace>`. To list registry namespaces, run `ibmcloud cr namespaces`.</td>
    </tr>
    </tbody></table>
    
    For example, create a policy for `codeengine-myproj-id` service ID with the role of `Reader`:
    
    ```
    ibmcloud iam service-policy-create codeengine-myproj-id --roles Reader --service-name container-registry
    ```
    {: pre}
    
3. Create a custom service policy to allow access to `iam-identity` service so that {{site.data.keyword.codeengineshort}} can retrieve the API key for your service ID.

    ```
    ibmcloud iam service-policy-create <service_ID> --roles <service_access_role> --service-name iam-identity [--region <IAM_region>] [--resource-type namespace --resource <registry_namespace>]
    ```
    {: pre}
    <table summary="The columns are read from left to right. The first column has the parameter of the command. The second column describes the parameter.">
    <caption>Understanding this command's components</caption>
    <col width="25%">
    <thead>
    <th>Component</th>
    <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;service_ID&gt;</em></code></td>
    <td>Required. Replace with the `codeengine-<project_name>-id` service ID that you previously created in step 1.</td>
    </tr>
    <tr>
    <td><code>--roles <em>&lt;platform_access_role&gt;</em></code></td>
    <td>Required. Enter the platform access role that you want to scope the service ID access to. Possible values are `Administrator`, `Editor`, `Operator`, and `Viewer`. Your service ID requires `Operator` or higher. </td>
    </tr>
    <tr>
    <td><code>--service-name <em>iam-identity</em></code></td>
    <td>Required. Enter `iam-identity` to create an IAM policy for IAM identify services.</td>
    </tr>
    </tbody></table>
    
    For example, create a policy for `codeengine-myproj-id` service ID with the role of `Operator`:
    
    ```
    ibmcloud iam service-policy-create codeengine-myproj-id --roles Operator --service-name iam-identity
    ```
    {: pre}

4. Create an API key for the service ID. Name the API key similar to your service ID, and include the service ID that you previously created, `codeengine-<project_name>-id`. Be sure to give the API key a description that helps you retrieve the key later.

    ```
    ibmcloud iam service-api-key-create codeengine-<project_name>-key codeengine-<project_name>-id --description "API key for service ID <service_id> for Code Engine <project_name>"
    ```
    {: pre}
    
    For example, create a key called `codeengine-myproj-key` for the `codeengine-myproj-id` service ID with a description of `API key for service ID codeengine-myproj-id for Code Engine myproj`:
    
    ```
    ibmcloud iam service-api-key-create codeengine-myproj-key codeengine-myproj-id --description "API key for service ID codeengine-myproj-id for Code Engine myproj"
    ```
    {: pre}
    
    **Output**
    
    ```
    Please preserve the API key! It cannot be retrieved after it's created.

    Name          codeengine-myproj-key   
    Description   API key for service ID codeengine-myproj-id for Code Engine myproj   
    Bound To      crn:v1:bluemix:public:iam-identity::a/1bb222bb2b33333ddd3d3333ee4ee444::serviceid:ServiceId-ff55555f-5fff-6666-g6g6-777777h7h7hh   
    Created At    2019-02-01T19:06+0000   
    API Key       i-8i88ii8jjjj9jjj99kkkkkkkkk_k9-llllll11mmm1   
    Locked        false   
    UUID          ApiKey-222nn2n2-o3o3-3o3o-4p44-oo444o44o4o4   
    ```
    {: screen}
    
   You won’t be able to see this API key again, so be sure to record it in a safe place.
   {: important}
   
 Now that you have your access policies in place for your service ID and your API key created, you can [add access to {{site.data.keyword.codeengineshort}}](#add-registry-access-ce) in order to pull images from your container registry.
