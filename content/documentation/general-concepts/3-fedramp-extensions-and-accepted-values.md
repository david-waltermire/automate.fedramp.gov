---
title: FedRAMP Extensions and Accepted Values
weight: 150
---
# FedRAMP Extensions and Accepted Values

The core OSCAL syntax is designed to represent cybersecurity
information that is common to any organization and compliance framework.
They recognized that each framework and organization may have unique
needs. Instead of trying to provide a language that meets each of those
unique needs, OSCAL provides organizations the ability to tailor OSCAL to
address specific needs.

{{<callout>}}
_A summary of the FedRAMP extensions and accepted values appears in the FedRAMP OSCAL Registry._
{{</callout>}}

FedRAMP has tailored OSCAL by specifying:

-   **Extensions**: allow FedRAMP's OSCAL-based content to capture
    information that is not available in the core OSCAL syntax.

-   **Accepted Values**: For many fields, FedRAMP specifies a
    case-sensitive set of accepted values. Only these values are
    recognized by FedRAMP processing tools.

## FedRAMP Extensions

There are several pieces of information required in FedRAMP templates
that cannot be modeled using the OSCAL core syntax. NIST wanted to limit
the core OSCAL syntax to those elements that are universal across most
cybersecurity frameworks. They designed OSCAL to be extended where
unique needs existed.

{{<callout>}}
_All FedRAMP extensions include a namespace (ns) flag set to `http://fedramp.gov/ns/oscal`._
{{</callout>}}

NIST allows organizations to extend OSCAL anyplace `prop` fields or `part`
assemblies exist in the core syntax. (Please note, there are currently
no part assemblies in the SSP, SAP, SAR, or POA&M.) There are two
fundamental requirements for extending OSCAL:

-   The organization must establish a unique namespace (`ns`) identifier,
    such as (`ns="http://domain.tld/ns/oscal"`), and use it to
    consistently tag all `prop` and `part` extensions from that
    organization.

-   The organization is responsible for defining, managing, and
    communicating all names (`name="scan-type"`) defined and tagged with
    the above namespace identifier.

OSCAL's core `prop` assemblies have no `ns` flag. If an `ns` flag is
present, it is an organization-defined extension. This allows each
industry standards body or organization to create their own extensions
in their own name space without concern for overlapping names. The above
approach ensures two different organizations can create their
own extensions without concern for reusing the same name values.

All FedRAMP extensions must have a namespace (`ns`) flag set to `http://fedramp.gov/ns/oscal`.

For example, if the core OSCAL syntax has a `status` field, but both
FedRAMP and the payment card industry (PCI) require their own
framework-specific status field, each may define an extension with the
`name="status"` and assign their own `ns` flag. This results in three
possible status fields as follows:

#### OSCAL User Representation

{{< highlight xml "linenos=table" >}}
  <!-- There is no @ns, so this is core OSCAL syntax -->
  <prop name="status" value="active" />
{{< /highlight >}}

#### XPath Query
{{< highlight xml "linenos=table" >}}
  //prop[@name="status"][not(@ns)]
{{< /highlight >}}

**When searching an OSCAL file for a prop or prop extension that is
part of the core OSCAL syntax, developers must filter out any with an ns
flag using the syntax above.**

#### FedRAMP Status Representation                                           
{{< highlight xml "linenos=table" >}}
  <prop name="status" ns="http://fedramp.gov/ns/oscal" value="FedRAMP Status" /> 
{{< /highlight >}}

#### XPath Query
{{< highlight xml "linenos=table" >}}
  //prop[@name="status"][@ns="http://fedramp.gov/ns/oscal"]
{{< /highlight >}}

#### (Possible) PCI Status Representation
{{< highlight xml "linenos=table" >}}
  <prop name="status" ns="https://pcisecuritystandards.org/ns/oscal"  value="PCI Status" />
{{< /highlight >}}

#### XPath Query
{{< highlight xml "linenos=table" >}}
  //prop[@name="status"][@ns="https://pcisecuritystandards.org/ns/oscal"]
{{< /highlight >}}

This is an example, and is not intended to represent an actual PCI
extension.

Tool developers must always refer to extensions using **both** the `name`
and `ns` flags as a pair.

All FedRAMP extensions will appear as:
{{< highlight xml "linenos=table" >}}
  <prop name="____" ns="http://fedramp.gov/ns/oscal" value="Value"/>
{{< /highlight >}}

**NOTE:** The catalog and profile OSCAL models also allow the `part`
assembly to be used for extensions. This is not currently the case for
the OSCAL SSP, SAP, SAR, or POA&M.

**FedRAMP extensions are cited in relevant portions of this document and
summarized in the FedRAMP OSCAL Registry.**


## FedRAMP Conformity Tagging

FedRAMP collaborated with NIST to address the ambiguities in OSCAL
syntax necessitating conformity tags.

### OSCAL and FedRAMP-Defined Identifiers and Accepted Values

OSCAL now defines *allowed values* in a way that supersedes FedRAMP's
separate handling of *defined identifiers* and *accepted values*. To
better align with OSCAL, FedRAMP has also shifted to this approach.
Further, many FedRAMP *defined values* are now recognized by OSCAL as
part of the core OSCAL syntax.

Any remaining *defined identifiers* or *accepted values* are enumerated
in the FedRAMP OSCAL registry as *allowed values*.

### OSCAL and FedRAMP Allowed Values

To facilitate consistent processing, the value for property names,
annotation names, and some field values is limited to a list of
*case-sensitive* allowed values. In many instances, OSCAL defines allowed
values, which are enforced by OSCAL-based syntax validation mechanisms.

In some cases, FedRAMP defines or adds allowed values specific to
FedRAMP ATO processing. Where defined, only these values are recognized
by FedRAMP processing tools.

For example, every control requires an implementation status. FedRAMP
only accepts one of five possible responses for this status, which must
be provided using one of the specified choices.

**FedRAMP allowed values are cited in relevant portions of each
guidebook and summarized in the FedRAMP OSCAL Registry.**

{{<callout>}}
_***Revised FedRAMP Registry Approach***<br/>The FedRAMP OSCAL Registry currently uses the draft OSCAL Extensions syntax and is offered in XML and JSON formats. This enables tools to be extension-aware._

- _[XML Version](https://github.com/GSA/fedramp-automation/raw/master/dist/content/rev5/resources/xml/FedRAMP_extensions.xml)_
- _[JSON Version](https://raw.githubusercontent.com/GSA/fedramp-automation/master/dist/content/rev5/resources/json/FedRAMP_extensions.json)_

FedRAMP is actively working on implementing a Metaschema-based approach to document all FedRAMP OSCAL extensions and constraints. The FedRAMP OSCAL extensions and constraints will be provided in XML, JSON, and YAML formats, and will replace the current FedRAMP Registry. FedRAMP also plans to provide a human-readable version of its OSCAL extensions and constraints in the future.
{{</callout>}}
