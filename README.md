# Template repository for Jupyter workshops on Surf Research Cloud
Zenodo DOI badge
![Static Badge](https://img.shields.io/badge/Python-black?style=flat-square&logo=python&logoColor=white&labelColor=gray&color=yellow)
![Static Badge](https://img.shields.io/badge/jupyter-blue?style=flat-square&logo=jupyter&logoColor=white&labelColor=gray&color=orange)
![Static Badge](https://img.shields.io/badge/MIT%20License%20-blue?style=flat-square)

## Introduction
This repository is a template for creating a Jupyter workshop set up on the Surf Research Cloud. The repository contains a folder structure to organize the workshop material, a GitHub Actions workflow to automatically compress the workshop material, and an ansible playbook to automatically transfer the workshop material to the digital workspaces on the Surf Research Cloud. An example workshop that uses this template can be found [here](https://github.com/UtrechtUniversity/gis-python-power).

## Steps to create a workshop (<60 minutes)

1. Create a new repository using this template repository.
2. Add the workshop material to the `workshop-material` folder.
3. Update the `environment.yml` file with the required packages.
4. Optionally: Update the `README.md` file with the workshop information.
5. Create a 'component' and 'catalog item' in the [Surf Research Cloud portal](https://portal.live.surfresearchcloud.nl/), [Instructions below](#creating-a-component-and-catalog-item-in-the-surf-research-cloud-portal).

## Deployment

- Participants of the workshop can be invited to SRAM via email to give them temporary access to the virtual environments.
- Creating a workspace takes less than 5 minutes per workspace + 15 minutes deployment time.

## Repository materials

- The workshop material can be edited found in the  **workshop-material folder**. It contains the jupyter notebooks as well as the data used for the workshop.
- The folder **playbooks** is exclusively dedicated for automated transfer of the course materials to the digital workspaces on SURF Research Cloud. It is automatically updated by the GitHub Actions workflow, so no need to make changes to this folder.
- The `environment.yml` file describes the packages that are required to run the notebooks. The file is used to create a python kernel on SURF Research Cloud that has all the packages installed. By default the kernel will be called `workshop-kernel` unless you choose a different name.
- The `.github` folder contains a GitHub Actions workflow that will automatically update the zipped `workshop-material.zip` whenever there are changes to the `workshop-material` folder.

```python
|   .gitignore
|   CITATION.cff
|   environment.yml
|   LICENSE
|   README.md
|   
+---.github
|   \---workflows
|           update-course-materials.yml
|           
+---playbooks
|   |   transfer_workshop_material.yml
|   |   
|   \---roles
|       \---transfer_workshop_material
|           \---tasks
|                   main.yml
|                   
\---workshop_material
    +---data
    |       README.md
    |       
    \---notebooks
            Intro_Jupyter.ipynb
```

## Creating a component and catalog item in the Surf Research Cloud portal

First make sure that you have an SRAM collaboration for the workshop. For UU researchers, you can request a collaboration by contacting [RDM support](https://www.uu.nl/en/research/research-data-management/tools/software-and-computing/virtual-research-environments).
Secondly, add yourself to the `src_co_developer` group in the SRAM collaboration.

### Create a component
Follow the instructions [here](https://servicedesk.surf.nl/wiki/display/WIKI/Create+a+component) to create a component in the Surf Research Cloud portal. 
Use the url of your newly created repository as the source code repository. And for Path `playbooks/transfer_workshop_material.yml` as the playbook path.

### Create a catalog item
Follow the instructions [here](https://servicedesk.surf.nl/wiki/display/WIKI/Create+a+catalog+item) to create a catalog item in the Surf Research Cloud portal. For the components selection, select:

- SRC-OS
- SRC-CO
- SRC-Nginx
- SRC-External plugin
- jupyter
- Custom Packages
- Shared Working Directory
- AND your newly created component

**IMPORTANT: In Step 6 you need to Overwrite several parameter settings**:  
- For `requirements_file_repo_url` and `requirements_file_path` (both used by the Custom Packages component). Check the instructions for `Custom Packages` component [here](https://gitlab.com/rsc-surf-nl/plugins/plugin-custom-packages/-/blob/main/README.md) and use the url of your newly created repository for `requirements_file_repo_url` and `environment.yml` for `requirements_file_path`.  
- For `paths`	(used by Shared Working Directory)	fill `/scratch`

After creating the catalog item, you can now select the catalog item when creating a workspace in the Surf Research Cloud portal. Be aware that in order to use the environment, the user would need to select the `workshop-kernel` from the kernel dropdown in the Jupyter notebook.

## Contact

- [Geo Data Team](https://geo-data-support.sites.uu.nl/)  
- [Research Engineering team](https://www.uu.nl/research-engineering)
