Utility element to enable repeatable pip installs
=================================================

Set this element as a dependency to make the utility scripts available,
and then use them as appropriate.

## Usage
This element makes a number of scripts available for use in image building, and
performs actions to copy specified manifests into the image for use in building.

It also copies manifests generated during the build back to the build environment
during cleanup.

## bin
Utility scripts for use in other elements to create and reuse pip manifests,
as detailed in the usage section.

### get-pip-manifest
Echoes the name of the pip manifest file if one has been copied in, or just
returns.  The caller passes the name associated with their element, which
should be descriptive of the element, to get the correct value.

For example, the nova element calls `get-pip-manifest nova`.

The name of the element is transformed to conform with bash variable naming
rules, so any charaters that are not [A-Za-z0-9] are replaced with '\_'.

### use-pip-manifest
Uses the given manifest to perform the pip installs necessary.

Note that any development versions listed in the manifest are not reinstalled.

The reason for this is that development versions are expected to have been
installed from a source other than pypi or the mirror in use, and so development
versions will not be reinstallable without extra information.

The exact details with respect to this are for the relevant consuming element to
determine.

### write-pip-manifest
Calls pip freeze and writes the versions of all of the packages currently
installed to a manifest file.

The format of the manifest is the standard python requirements format that is
generated by the "pip freeze" command.

## extra-data.d

### 75-inject-pip-manifests
Copies any pip manifest specified in DIB\_PIP\_MANIFEST\_\* environment variables
into the image chroot environment.

## install.d

### 01-pip-manifest
Installs the scripts in this element into the image for later use by other elements.

