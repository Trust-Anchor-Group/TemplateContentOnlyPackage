Content Service Package
==========================

This repository provides a template solution that can be forked by developers who want to create a custom content-only package for the 
TAG Neuron(R). A Content-Only package contains no assemblies, and does not require restarting the Neuron(R) when installing the package. 
Program logic is implemented using client Javascript and [back-end server script](https://lab.tagroot.io/Script.md).

Steps to create a content-only service
-----------------------------------------

1.  Fork this repository
	* Follow naming conventions for repositories, to make the repository easy to find. A Tag Service running on the TAG Neuron(R) typically
	resides in a repository named `NeuronSERVICE`, where `SERVICE` is a short name for the service being implemented.
	* It has been assumed the repository will be cloned to `C:\My Projects\TemplateContentOnlyPackage`.

2.  Change the solution and project names, as well as the corresponding *manifest file* (see below).

3.  [Install a Neuron](https://lab.tagroot.io/Documentation/Neuron/InstallBroker.md) on your development machine, that you can use for debugging 
	and testing.

	* Since a Content-Only package does not require the Neuron(R) to stop during updates, the service can run while you develop.

	* If the project requires you to use *Neuro-Access* to login to show protected pages, your *development neuron* needs to add the
	XMPP addresses (JIDs) of the apps to its Roster. On published Neurons, this is not necessary, as they are federated and use their domain names
	to interconnect with other Neurons (such as the Neurons serving your *Neuro-Access* connections). But a developer Neuron(R) cannot
	do this, as they most probably lacks both domain names and public IP addresses. Communication in this case, is done using the XMPP
	Addresses (JIDs) of the Neurons, as clients to their parent Neurons. For communication to pass in this case, and not be rejected as
	invalid, the sender has to reside in the Neurons *Roster* (accessible from the Admin page, in the Communication section).
	
4.  Add a reference to the content files in the `gateway.config` file (see below) so the Neuron(R) can find the files while you work with them.
	You will need to restart the IoT Broker service for the changes to take effect.

5.  Add content files to the project, as required. Instructions and descriptions of the different content file types and how to work with them
	is made available in the content published by the template itself.

6.  Update the Manifest file so it contains all referenced content files and folders necessary to install service on a Neuron(R). You do
	not need to reference content files that are part of the Neuron(R) distribution itself.

7.  Create an installable package that can be distributed and installed on TAG Neurons.

8.  Test on a local development Neuron(R).

9.  Once it works, sign and distribute package on test Neurons, and later production Neurons.

10. Update project documentation for future developers, following documentation style of similar projects, for recognizability and ease of use.

11. Append template documentation with useful hints or information, if needed.

12. Provide a correct license for the repository.

## Installable Package

To create a package, that can be distributed or installed, you begin by creating a *manifest file*. The repository has a manifest file
called `ContentServiceTemplate.manifest`. It defines the content files and folders included in the package. You then use the 
`Waher.Utility.Install` and `Waher.Utility.Sign` command-line tools in the [IoT Gateway](https://github.com/PeterWaher/IoTGateway) repository, 
to create a package file and cryptographically sign it for secure distribution across the Neuron network. These tools are also available in 
the installation folder of the Neuron(R) distribution.

### Gateway.config

To simplify development, once the project is cloned, add a `FileFolder` reference to your repository folder in your 
[gateway.config file](https://lab.tagroot.io/Documentation/IoTGateway/GatewayConfig.md). This allows you to test and run your changes to 
Markdown, [back-end script](https://lab.tagroot.io/Script.md) and Javascript immediately, without having to synchronize the folder contents 
with an external host, or recompile or go through the trouble of generating a distributable software package just for testing purposes. 
Changes you make in .NET can be applied in runtime if you the *Hot Reload* permits, otherwise you need to recompile and re-run the application 
again.

Example of how to point a web folder to your project folder:

```
<FileFolders>
  <FileFolder webFolder="/TemplateContent" folderPath="C:\My Projects\TemplateContentOnlyPackage\Root\TemplateContent"/>
</FileFolders>
```

**Note**: Once the file folder reference is added, you need to restart the Neuron(R) for the change to take effect.

**Note 2**:  Once the Neuron(R) is restarted, the source for the files is taken from the new location. Any changes you make 
in the corresponding `ProgramData` subfolder will have no effect on what you see via the browser.

**Note 3**: This file folder is only necessary on your developer machine, to give you real-time updates as you edit the files in your
development folder. It is not necessary in a production environment, as the files are copied into the correct folders when the package 
is installed.
