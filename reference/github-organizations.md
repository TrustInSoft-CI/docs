# GitHub organizations

## Analyzing an organization project

[GitHub organizations](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-organizations) are shared GitHub accounts where businesses and open-source teams can collaborate on the same projects.

Projects owned by an organization are called organization projects. There are only two prerequisites to analyze such projects:

1. TrustInSoft CI must be [granted access](github-organizations.md#granting-access-to-trustinsoft-ci) to the organization owning the project. 
2. Only an organization [owner ](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/permission-levels-for-an-organization)\(admin\) can add such project to TrustInSoft CI. This permission will be required for any TrustInSoft CI operation on the project \(adding/deleting the project\) or its analyses \(canceling/restarting an analysis\).

Then, you just need to select the right organization in the [Add project page](https://ci.trust-in-soft.com/projects/add-project), select the project and follow the usual steps \(Add the tis.config file, Click **Start verifying**, etc.\):

![](../.gitbook/assets/image%20%28100%29.png)

As soon as the project is added, it will show up in the organization's dashboard page and be removed from the project selection list in Add project. 

Both the organization's members and owners will be able to access their organization's dashboard at any time, by selecting the organization name in the account selection dropdown in the navigation bar:

![](../.gitbook/assets/image%20%28140%29.png)

## Granting access to your organization

There are two ways to grant TrustInSoft CI access to your organization.

### **During sign-up**

On the GitHub authorization page, click **Grant** next to the organization:

![User &quot;guillaumemillot1&quot; owns &quot;guillaumeorg1&quot; and is member of &quot;paulorg1&quot;](../.gitbook/assets/image%20%2847%29.png)

### After sign-up

1. Load your [GitHub account settings page](https://github.com/organizations/TrustInSoft/settings/profile)
2. In the Personal settings, click **Applications**
3. Click **Authorized OAuth Apps**
4. Click **TrustInSoft CI**
5. Click **Grant** next to the organization

![](../.gitbook/assets/image%20%28160%29.png)

![](../.gitbook/assets/image%20%28118%29.png)

