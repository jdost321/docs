Registering in the OSG
======================

The OSG keeps a registry containing active projects, virtual organizations (VOs), resources, and resource
downtimes stored as as [YAML files](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html)
in the [topology GitHub repository](https://github.com/opensciencegrid/topology/).
This registry is used for [accounting data](https://gracc.opensciencegrid.org), contact information, and resource
availability, particularly if your site is part of the [World LHC Computing Grid](http://wlcg.web.cern.ch/) (WLCG).

Use this page to learn how to register information in the OSG.

Registration Requirements
-------------------------

The instructions in this document require the following:

- A [GitHub](https://github.com/) account
- A working knowledge of [GitHub collaboration](https://help.github.com/categories/collaborating-with-issues-and-pull-requests/)
- [OSG contact](#registering-contacts) registration

Registering Contacts
--------------------

The OSG keeps a database of contact information for resources, virtual organizations, and projects.
Before submitting any registrations in the topology repository, you must have an entry in the contact database with an
associated GitHub ID.
To register as an OSG contact or add your GitHub ID to your pre-existing contact entry, search for your name in the
[contact database](https://topology.opensciencegrid.org/miscuser/xml?) and choose one of the following:

- **If you find your name and see an associated `GitHub` account**, you can start submitting registrations!
- **If you find your name but do not see an associated `GitHub` account**, send an email to <mailto:help@opensciencegrid.org>
  with your GitHub ID.
- **If you cannot find your name**, send an email to <mailto:help@opensciencegrid.org> with the following information:
    - Full name
    - Primary email address
    - GitHub ID
    - Description of your OSG affiliation, e.g. FermiGrid site administrator, senior scientist for the DUNE experiment,
      etc.
    - Contact information of site, virtual organization, or project sponsor to prove your affiliation

Registering Resources
---------------------

An OSG resource is a host that provides grid services, e.g. Compute Elements, storage endpoints, or perfSonar hosts.
See the full list of services that should be registered in the OSG topology
[here](https://github.com/opensciencegrid/topology/blob/master/topology/services.yaml).

OSG resources are stored under a hierarchy of facilities, sites, and resource groups, defined as follows:

| Level          | Definition                                                                                                                                                             |
|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Facility       | The institution or company where your resource is located, e.g. `University of Wisconsin`                                                                              |
| Site           | Smaller than a facility; typically represents an academic department, research group, or a computing center, e.g. `CHTC` for the Center for High Throughput Computing. |
| Resource Group | A logical grouping of resources, e.g. you may group together resources that serve your Slurm cluster under a resource group named `CHTC-Slurm-HPC`                     |
| Resource       | A host that provides grid services, e.g. Compute Elements, storage endpoints, or perfSonar hosts.                                                                      |

OSG resources are stored in the GitHub repository as YAML files under a directory structure that reflects the above
hierarchy, i.e. `topology/<FACILITY>/<SITE>/<RESOURCE GROUP>.yaml` from the
[root of the topology repository](https://github.com/opensciencegrid/topology/tree/master/).

### Searching for resources ###

Whether you are registering a new resource or modifying an existing resource, start by searching for the FQDN of your
host to avoid any duplicate registrations:

1. Open the [topology repository](https://github.com/opensciencegrid/topology/tree/master/) in your browser.

1. Search the repository for the FQDN of your resource wrapped in double-quotes using the GitHub search bar
   (e.g., `"glidein2.chtc.wisc.edu"`):

    ![GitHub search](/img/registration/github-search.png)

    - **If the search doesn't return any results**, skip to [these instructions](#new-resources) for registering a new
      resource.
    - **If the search returns a single YAML file**, open the link to the YAML file and skip to
      [these instructions](#modifying-existing-resources) for modifying an existing resources.
    - **If the search returns more than one YAML file**, please [contact us](#getting-help).

### New resources ###

To register a new resource, follow the instructions below:

1. If you haven't already, verify that the FQDN of your resource is not [already registered](#registering-resources)

1. Choose the names of your facility, site, and resource group, ensuring that the names match any pre-existing
   facilities, sites, or resource groups (including case and spaces).
   Follow the instructions below, replacing instances of `<FACILITY>`, `<SITE>`, and `<RESOURCE GROUP>` with the
   corresponding names that you chose above:

    - If your resource group already exists under your facility and site, open the following URL in your browser:

            https://github.com/opensciencegrid/topology/edit/master/<FACILITY>/<SITE>/<RESOURCE GROUP>.yaml

        For example, to add a resource to the `CHTC` resource group for the `CHTC` site at the `University of
        Wisconsin`, open the following URL:

            https://github.com/opensciencegrid/topology/edit/master/topology/University of Wisconsin/CHTC/CHTC.yaml

    - If any of your facility, site, or resource group do not exist, open the following URL in your browser:

            https://github.com/opensciencegrid/topology/new/master?filename=topology/<FACILITY>/<SITE>/<RESOURCE GROUP>.yaml

        For example, to create a `CHTC-Slurm-HPC` resource group for the Center for High Throughput Computing (`CHTC`)
        at the `University of Wisconsin`, open the following URL:

            https://github.com/opensciencegrid/topology/new/master?filename=topology/University of Wisconsin/CHTC/CHTC-Slurm-HPC.yaml

1. Make changes with the [GitHub file editor](https://help.github.com/articles/editing-files-in-your-repository/) using
   the [resource group template](https://github.com/opensciencegrid/topology/blob/master/template-resourcegroup.yaml)
   as a guide.

    !!! note
        Make sure the info you add matches the formatting and indentation of the template file.
        In particular, make sure no additional indentation gets added when pasting in the new data.

1. Submit your changes as a pull request, providing a descriptive commit message. For example:

        Adding a new compute element to the CHTC

### Modifying existing resources ###

To modify an existing resource, follow these instructions:

1. Find the resource that you would like to modify by [searching GitHub](#searching-for-resources), and open the link to
   the YAML file.

1. Click the branch selector button next to the file path and select the `master` branch.

    ![GitHub branch selection](/img/registration/switch-branch.png)

1. Make changes with the [GitHub file editor](https://help.github.com/articles/editing-files-in-your-repository/) using
   the [resource group template](https://github.com/opensciencegrid/topology/blob/master/template-resourcegroup.yaml)
   as a guide.

    !!! note
        Make sure the info you add matches the formatting and indentation of the template file.
        In particular, make sure no additional indentation gets added when pasting in the new data.

1. Submit your changes as a pull request, providing a descriptive commit message. For example:

        Updating administrative contact information for CHTC-glidein2

### Retiring resources ###

To retire an already registered resource, choose one of the following, depending on the resource's service(s):

- If the resource contains a `CE` (Compute Element) or `Submit Node` (GlideinWMS frontend) service, set `Active: false`
  within resource group's YAML file.
  For example, to remove the resource GLOW, edit the file `topology/University of Wisconsin/GLOW/GLOW.yaml`, setting
  `Active: false`:

        ...
        Production: true
        Resources:
          GLOW:
            Active: %RED%false%ENDCOLOR%
            ...
            Services:
              CE:
                Description: Compute Element
                Details:
                  hidden: false

    You may have to add the `Active` attribute it if does not already exist within the resource definition.

- If the resource does not contain a `CE` (Compute Element) or OSG submitter service, you may remove the resource
  completely.
  If there are no more resources in the resource group, you may remove the entire resource group file.


Registering Resource Downtimes
------------------------------

Resource downtime is a period of time for which one or more of the grid services of a registered resource are
unavailable.
Downtimes are stored in YAML files alongside the resource group YAML files as described [here](#registering-resources),
i.e. `topology/<FACILITY>/<SITE>/<RESOURCE GROUP>_downtime.yaml` from the
[root of the topology repository](https://github.com/opensciencegrid/topology/tree/master/).
To register a new downtime for a registered resource, follow the instructions below:

1. Open the [topology repository](https://github.com/opensciencegrid/topology/tree/master/) in your browser.

1. Search the repository for the FQDN of your resource wrapped in double-quotes using the GitHub search bar
   (e.g., `"glidein2.chtc.wisc.edu"`):

    ![GitHub search](/img/registration/github-search.png)

    - **If the search returns a single YAML file**, note the name of the facility, site, and resource group and continue
      to the next step.
    - **If the search doesn't return any results**, your resource is not registered.
      Follow the [resource registration](#registering-resources) instructions before registering a downtime.
    - **If the search returns more than one YAML file**, please [contact us](#getting-help).

1. Open the following URL in your browser using the facility, site, and resource group names to replace `<FACILITY>`,
   `<SITE>`, and `<RESOURCE GROUP`>, respectively:

        https://github.com/opensciencegrid/topology/edit/master/topology/<FACILITY>/<SITE>/<RESOURCE GROUP>_downtime.yaml

    If the above URL returns a 404, open this link instead:

        https://github.com/opensciencegrid/topology/new/master?filename=topology/<FACILITY>/<SITE>/<RESOURCE GROUP>_downtime.yaml

1. Make changes with the [GitHub file editor](https://help.github.com/articles/editing-files-in-your-repository/) using
   the [downtime template](https://github.com/opensciencegrid/topology/blob/master/template-downtime.yaml) as a
   guide.

    !!! note
        Make sure the info you add matches the formatting and indentation of the template or the other downtime entries
        in the file.
        In particular, make sure no additional indentation gets added when pasting in the new data.

1. Submit your changes as a pull request, providing a descriptive commit message. For example:

        Adding downtime for CHTC-glidein2 for regular maintenance

Registering Virtual Organizations
---------------------------------

Virtual Organizations (VOs) are sets of groups or individuals defined by some common cyber-infrastructure need.
This can be a scientific experiment, a university campus or a distributed research effort.
A VO represents all its members and their common needs in a grid environment.
A VO also includes the group’s computing/storage resources and services.
For more information about VOs, see [this page](https://opensciencegrid.org/about/organization#virtual-organizations).

!!! note
    Before submitting a registration for a new VO, please [contact us](#getting-help) describing your organization's
    computing needs.

VO information is stored as YAML files in the `virtual-organizations` directory of the
[topology repository](https://github.com/opensciencegrid/topology/tree/master).
To modify a VO's information or register a new VO, follow the instructions below:

1. Open the [topology repository](https://github.com/opensciencegrid/topology/tree/master/virtual-organizations) in your
   browser.

1. If you see your VO in the list, open the file and continue to the next step.
   If you do not see your VO in the list, click `Create new file` button:

    ![GitHub file creation](/img/registration/create-file.png)

    In the new file dialog, enter `<VO>.yaml`, replacing `<VO>` with the name of your VO.

1. Make changes with the [GitHub file editor](https://help.github.com/articles/editing-files-in-your-repository/) using
   the [VO template](https://github.com/opensciencegrid/topology/blob/master/template-virtual-organization.yaml)
   as a guide.

    !!! note
        Make sure the info you add matches the formatting and indentation of the template file.
        In particular, make sure no additional indentation gets added when pasting in the new data.

1. Submit your changes as a pull request, providing a descriptive commit message. For example:

        Updating contact information for the GLOW VO

Registering Projects
--------------------

!!! note
    Before submitting a registration for a new project, please [contact us](#getting-help) describing your organization's
    computing needs.

Project information is stored as YAML files in the `projects` directory of the
[topology repository](https://github.com/opensciencegrid/topology/tree/master).
To modify a VO's information or register a new VO, follow the instructions below:

1. Open the [topology repository](https://github.com/opensciencegrid/topology/tree/master/projects) in your browser.

1. If you see your project in the list, open the file and continue to the next step.
   If you do not see your project in the list, click `Create new file` button:

    ![GitHub file creation](/img/registration/create-file.png)

    In the new file dialog, enter `<PROJECT>.yaml`, replacing `<PROJECT>` with the name of your project.

1. Make changes with the [GitHub file editor](https://help.github.com/articles/editing-files-in-your-repository/) using
   the [VO template](https://github.com/opensciencegrid/topology/blob/master/template-projects.yaml)
   as a guide.

    !!! note
        Make sure the info you add matches the formatting and indentation of the template file.
        In particular, make sure no additional indentation gets added when pasting in the new data.

1. Submit your changes as a pull request, providing a descriptive commit message. For example:

        Updating contact information for the Mu2e project

Getting Help
------------

To get assistance, please use the [this page](/common/help).