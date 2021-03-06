Editors Guide
=============
If you're not sure how to do something please ask on edam@elixir-dk.org.

For EDAM Core Developers
------------------------

Modifying GitHub main repo.
^^^^^^^^^^^^^^^^^^^^^^^^^^^
`EDAM Core Developers <http://edamontologydocs.readthedocs.io/en/latest/governance.html>`_ can edit the main repository.  The workflow is:

1. Get the "editing token" 

   - Contact edam-core@elixir-dk.org and claim the "editing token" after first checking that it is not currently taken :)
   - Say what you are doing, why, and about how long it will take

2. Update your local repo with the latest files from the GitHub master:

    ``git pull``
   
   If you've not already done so, you will first need to clone the master repo:

    ``git clone https://github.com/edamontology/edamontology.git``

3. Make and commit your local changes. You **must** be working with the latest "dev" version, *e.g.* ``EDAM_1.5_dev.owl``. You should leave the version number unchanged, *i.e.* should not need to add any new files to the repo.

   - Check your changes and that the OWL file looks good in Protege
   - Ensure the ``next_id`` attribute is updated
   - Ensure that ``oboOther:date`` is updated to the current GMT/BST before the commit
   - Add the edited file to the commit
   
      ``git add <filepath>``
   - Commit your local changes, including a concise but complete summary of the major changes:
   
      ``git commit -m ��commit message here��``

4. Push your changes to the GitHub master:

    ``git push origin``

**Please provide a meaningful commit message so that we can easily generate the ChangeLog upon next release**

5. Release the editing token for the other developers:

   - Contact edam-core@elixir-dk.org and release the "editing token" .
   - Summarise what you actually did and why.

Creating a new official EDAM release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
From January 2016, EDAM tries to follow a bi-monthly release cycle to this schedule:

1.  First Wed of every month
   - EDAM team skype to discuss plans for this month.  Announcement (to edam-announcence) including short summary of plans, invitation for suggestions.
2.  Last Mon of every month
   - Announcement (to edam-announcence) saying that release is immiment, invitation for last-minute suggestions.
3.  Last Wed of every month
   - Complete the work for the release.  Make the release.  Ensure it works in BioPortal, OLS, and in bio.tools.
4.  Last Fri of every month
   -  Announcee the release, incuding summary of changes.

Before creating a new release, please make sure you have the approval of leader of EDAM core-dev, and that the `changelog.md <https://github.com/edamontology/edamontology/blob/master/changelog.md>`_ and `changelog-detailed.md <https://github.com/edamontology/edamontology/blob/master/changelog-detailed.md>`_ files are up-to-date with the changes of the new release.  See section below on creating the ChangeLog files.  Once you're clear to go, do the following:

1. Update your local version of the repository:

    ``git pull``
2. Assuming you are releasing version n+1, n being the current version:

   - you initially have ``EDAM_dev.owl`` in the repository
   - make sure to update ``oboOther:date`` in this file
   - copy the file ``EDAM_dev.owl`` to ``releases/EDAM_n+1.owl``

    ``cp EDAM\_dev.owl releases/EDAM_n+1.owl``
    ``git add releases/EDAM\_n+1.owl``

   - modify the ``doap:version`` property to **n+1** in ``releases/EDAM_n+1.owl`` and to **n+2_dev** in ``EDAM_dev.owl``
   
   - commit and push your changes

    ``git commit -a``

    ``git push origin``

3. Update the file names of ``web/page_x.html`` and ``relations-and-properties_x.html``: update the version number to **n+1** (in file name, and multiple places in the contents), and also update the last update date in ``web/page_x.html``.
4. Update the `detailed changelog <https://github.com/edamontology/edamontology/blob/master/changelog-detailed.md>`_ by running `Bubastis <http://www.ebi.ac.uk/efo/bubastis/>`_ to compare the release against the previous version.
5. Update the `changelog <https://github.com/edamontology/edamontology/blob/master/changelog.md>`_ with a summary of the major changes.
6. Create the release on GitHub (use the `_draft a new release_ <https://github.com/edamontology/edamontology/releases/new>`_ button of the `_releases_ <https://github.com/edamontology/edamontology/releases>`_ tab).
7. Update http://edamontology.org.
8. Submit this new release to BioPortal.  OLS will pull the file automatically from edamontology.org every night.
9. Close GitHub issues labelled *done - staged for release*. 
10. Announce the new release on Twitter and mailing lists (edam-announce@elixir-dk.org, edam@elixir-dk.org) including thanks and a summary of changes.
11. Help apps that implement EDAM to update to the new version. In particular `bio.tools <http://bio.tools>`_.


Editing the ChangeLog
^^^^^^^^^^^^^^^^^^^^^
The ChangeLog includes:

1. `changelog <https://github.com/edamontology/edamontology/blob/master/changelog.md>`_ - a summary of the major changes and what motivated them
2. `detailed changelog <https://github.com/edamontology/edamontology/blob/master/changelog-detailed.md>`_ - fine-grained details obtained using `Bubastis <http://www.ebi.ac.uk/efo/bubastis/>`_

The changelog should include:

1. (as 1st paragraph) an "executive summary" suitable for consumption by technical managers, describing the motivation for major changes, including *e.g.* requests at recent hackathons, requests via GitHub, strategic directions etc.
2. summary of changes distilled from the output of `Bubastis <http://www.ebi.ac.uk/efo/bubastis/>`_  (see below). 
3. summary of GitHub commit messages.  **please ensure meaningful commit messages are provided on every commit**

Some hacking of bubastis output is needed to identify (at least):
  - number of new concepts
  - number of deprecations
  - summary of activity, i.e. in which branches was most work focucssed ?



For Editors 
-----------


General guidelines
^^^^^^^^^^^^^^^^^^

1. As much as you can, try to make atomic changes and commit them independently. this improves greatly traceability in the long term
2. Make trivial modifications using a text editor if possible, rather than Prot��g��, because the actual modification is not hidden in haystack of Prot��g�� reformattings 
3. Check and double-check your changes: errors are hard to track and fix later

Adding concepts
^^^^^^^^^^^^^^^

When adding new terms, you **MUST** specify the following (attributes are in parenthesis):

1. Correct concept URI, i.e. in the right namespace and with the latest ID
2. Preferred term (``rdfs:label``)
3. Definition (``oboInOwl:hasDefinition``) 
4. Parent concept (``rdfs:subClassOf``)
5. Current dev version into ``created_in`` : type a value e.g.  ``1.5``
6. The ��edam�� subset (``oboInOwl:inSubset``): in Protege, pick (don't type!) the value of ``edam``
7. The branch subset (``oboInOwl:inSubset``): pick one of ``topic``, ``data``, ``format`` or ``operation``
8. Any specialised subset (pick as above, only if required) 
9. The next ID ontology attribute (``next_id``)

Note that :

- The **preferred label** should be a short name or phrase in common use.
- Consider providing common **synonyms** of the term:

   - Exact synonym (``oboInOwl:hasExactSynonym``) - bog-standard synyonsm
   - Narrow synonym (``oboInOwl:hasNarrowSynonym``) - specialisms of the term
   - Broad synonym (``oboInOwl:hasBroadSynonym``) - generalisations of the term

NB: Do **not** include American spellings or case variants as synonyms.

- The **definition** should be a concise and lucid description of the concept, without acronyms, and avoiding jargon.
- Peripheral but important information can go in the **comment** (``rdfs:comment``).

In addition, for **Format** concepts, please specify:

1. The Data concept which the format applies to : define this relation in Protege using the pattern *Format is_format_of some Data*
2. The URL of the format documentation, if available (``Documentation`` attribute) : in Protege, type a URL using the Protege IRI editor.  

In addition, for **Identifier** concepts, specify:

1. The Data concept which the identifier applies to : define this relation in Protege using the pattern 'Identifier is_identifier_of some Data'  
2. The regular expression defining valid values of that identifier (``Regular expression``) : type the regex into the Protege 'Constant" editor 

In addition, for **Topic** concepts, specify:

1. The corresponding Wikipedia page that exact matches the term (``Documentation`` attribute) : in Protege, type a URL using the IRI editor.  This method will change when we eventually link via Wikidata.




Deprecating concepts
^^^^^^^^^^^^^^^^^^^^ 
When deprecating concepts, you _**MUST**_ specify the following:

1. Current dev version into ``obsolete_since``.
2. The ��obsolete�� subset (``oboInOwl:inSubset``): pick ``obsolete``.
3. The ``deprecated`` attribute (``owl:deprecated``): type the value of ``true``.
4. The alternative ��replacement�� term to firmly use (``oboInOwl:replacedBy``), or to consider when less certain (``oboInOwl:consider``): pick a concept.
5. Set the parent concept (``rdfs:subClassOf``) to the ``ObsoleteClass``. 
6. Remove all other class annotations (subsets, comments, synonyms etc.) and axioms (including parent concepts).


Ensuring logical consistency
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Before committing changes, to ensure logical consistency of EDAM, please do the following within Protege:

1. Click *Reasoner->Hermit*
2. Click *Reasoner->Start reasoner* (it may take a few seconds)
3. In the *Entities* tab, select the *Class hierarchy (inferred) tab*
4. Select the *nothing* branch

If nothing (no classes) are shown under the *nothing* branch, then all is well.  If one or more classes are shown, then there is a logical inconsistency which must be fixed.  You might see lots of classes, but usually the problem is in one or a few classes.  

Common problems include:

- classes assigned as a ``subClass`` of some deprecated term
- end-point of relations are in the wrong branch, e.g. `class has_topic some operation`.  These can easily occur if you use the *Class expression editor* in Protege to define such axioms: this is NOT EDAM namespace aware, and in cases where a concept with the same preferred label exists in both classes, can easily pick the wrong one.

The problems are easily fixed within Protege: ask on the mailing list if you're not sure how.  Finally, do not be tempted to click *Reasoner->Synchronise reasoner* between changes: it tends to hang Protege.  Instead, use *Reasoner->Stop reasoner* than *Reasoner->Start reasoner*.

Continuous Integration
----------------------
Every modification on the ontology pushed to GitHub triggers an automated test in Travis CI. For now, it only checks a few rules using the `edamxpathvalidator tool <https://github.com/edamontology/edamxpathvalidator>`_. The Travis-CI website shows you the current status `here <https://travis-ci.org/edamontology/edamontology>`_. The fact that the continuous integration task succeeds does not guarantee that it there are no remaining bugs, but a failure means that you must take action to correct the problem, either fix it, fix the ``edamxpathvalidator`` program, or ask the mailing list if you're unsure.

Modifications in a GitHub fork
------------------------------
GitHub makes it possible for any developer (even if you are not a ��core developer��) to make modifications in a copy of EDAM and suggest these modifications are included in the original.  Please note that we discourage using this mechanism for large modifications made using Prot��g��, because merging OWL files which have been reformatted by Protege is notoriously unreliable (see ��Best practices for edition�� below). If you get an agreement from the core developers to make large modifications in Protege, we can provide you a core developer status on a temporary basis. This access will be removed once the task is accomplished.

The workflow is:

- Fork the edamontology repository in your own account.
- Make the modifications you want to suggest for inclusion in EDAM in this forked repository.
- Open pull requests for each modification you make.

Please make sure to:

- Keep your forked repository synchronized with the core repository, to avoid inconsistencies.
- Make sure to follow the "Best practices for edition" below.



