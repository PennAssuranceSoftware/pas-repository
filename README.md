## About

This project is the master for the Amdatu release repository. Its contents is
mirrored at `repository.amdatu.org`.

## Release HOWTO

In order to create releases for Amdatu, you need the following prerequisites:

1. A valid account and sufficient rights on the "Amdatu-repository" repository
on BitBucket;
2. A local copy of the "Amdatu-repository" repository on your machine.

In addition, you need to make sure your Bndtools-based workspace is configured
correctly. In the `cnf/ext/repositories.xml` make sure you've defined the
following repositories (be sure to use line continuations!):

	aQute.bnd.deployer.repository.LocalIndexedRepo;name='Amdatu releases (local)';local=${workspace}/../amdatu-repository/release
	aQute.bnd.deployer.repository.LocalIndexedRepo;name='Amdatu snapshots (local)';local=${workspace}/../amdatu-repository/snapshot

As you can see, this assumes that, next to your project, you've checked out a
copy of the 'amdatu-repository' project in a folder next to your project.

To make a release, the following steps should be taken:

1. Release the relevant bundles from Eclipse using Bndtools directly into the
**snapshot** repository of Amdatu. This will "stage" your bundles to be tested
before a final release will take place;
2. Generate an index for the **snapshot** repository (see "[Generating an
index](#genindex)");
3. Commit the changes to the repository, the changes will be automatically
synchronized to `repository.amdatu.org`;
4. Verify whether your staged snapshot bundles work as expected by using them
from the remote repository (see "[Using Amdatu repository](#useremoterepo)");
5. When verified correctly, *move* the bundle from the snapshot repository to
the **release** repository of Amdatu. No new build of the bundle is needed;
6. Regenerate the indexes for both the snapshot repository as well as the
release repository (see "[Generating an index](#genindex)");
7. Commit the changes. After a short while, the released bundle(s) are
available from `repository.amdatu.org`.

## Generating an index

To generate an index, you need to run the `genindex.sh` script found in the
`util` directory. It accepts *one* parameter: `-a` will generate the indexes
for *both* the release as well as the snapshot directory. Alternatively, you
can supply either *snapshot* or *release* to specifically generate an index
for that directory only.

For example, to generate an index for the snapshot directory, you run it as
follows:

    [localhost:~/]$ ./util/genindex.sh snapshot

After the index is generated, you can commit and push the new index to the main
repository on BitBucket.

## Using the Amdatu repository 

To use the remote release and snapshot repositories of Amdatu in Bnd(tools),
you need to add the following repositories to your `cnf/ext/repositories.xml`
file (be sure to use line continuations!):

    aQute.bnd.deployer.repository.LocalIndexedRepo;name='Amdatu Release Repository';locations='http://repository.amdatu.org/release/repository.xml'
    aQute.bnd.deployer.repository.LocalIndexedRepo;name='Amdatu Snapshot Repository';locations='http://repository.amdatu.org/snapshot/repository.xml'

After the workspace is refreshed, the "Repositories" view should now list two
new repositories: "Amdatu Snapshot Repository" and "Amdatu Release Repository".

