---
title: "A YANG Data Model for Flexi-Grid Optical Networks"
abbrev: "Flexi-Grid YANG"
category: std

docname: draft-ietf-ccamp-flexigrid-yang-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Routing"
workgroup: "CCAMP Working Group"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: "Common Control and Measurement Plane"
  type: "Working Group"
  mail: "ccamp@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ccamp/"
  github: "ietf-ccamp-wg/draft-ietf-ccamp-flexigrid-yang"
  latest: "https://ietf-ccamp-wg.github.io/draft-ietf-ccamp-flexigrid-yang/draft-ietf-ccamp-flexigrid-yang.html"

author:
  -
    name: Jorge E. Lopez de Vergara Mendez
    org: Naudit HPCN
    email: jorge.lopez_vergara@uam.es
  -
    name: Daniel Perdices Burrero
    org: Universidad Autonoma de Madrid
    email: daniel.perdices@uam.es
  -
    name: Daniel King
    org: Old Dog Consulting
    email: daniel@olddog.co.uk
  -
    name: Young Lee
    org: Samsung
    email: younglee.tx@gmail.com
  -
    name: Haomian Zheng
    org: Huawei Technologies
    email: zhenghaomian@huawei.com

contributor:
  -
    name: Oscar Gonzalez de Dios
    ins: O. Gonzalez de Dios
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com
  -
    name: Gabriele Galimberti
    email: ggalimbe56@gmail.com
  -
    name: Sergio Belotti
    org: Nokia
    email: sergio.belotti@nokia.com
  -
    name: Zafar Ali
    org: Cisco
    email: zali@cisco.com
  -
    name: Daniel Michaud Vallinoto
    org: Universidad Autonoma de Madrid
    email: daniel.michaud@estudiante.uam.es
  -
    name: Steven Hill
    org: MTN Group Technology
    email: Steven.Hill@mtn.com
  -
    name: Victor Lopez
    org: Nokia
    email: victor.lopez@nokia.com
  -
    name: Italo Busi
    org: Huawei Technologies
    email: italo.busi@huawei.com
  -
    name: Aihua Guo
    org: Futurewei
    email: aihuaguo.ietf@gmail.com

normative:
  ITU-T_G.694.1:
    title: "Spectral grids for WDM applications: DWDM frequency grid"
    author:
      org: ITU-T Recommendation G.694.1
    date: October 2020
    seriesinfo: ITU-T G.694.1
  ITU-T_G.872:
    title: Architecture of optical transport networks
    author:
      org: International Telecommunication Union
    date: January 2021
    seriesinfo: ITU-T G.872 Amendment 1

--- abstract

This document defines a YANG module for managing flexi-grid optical
networks.  The model defined in this document specifies a flexi-grid
traffic engineering database that is used to describe the topology of
a flexi-grid network.  It is based on and augments existing YANG
models that describe network and traffic engineering topologies.

The YANG data model defined in this document conforms to the Network
Management Datastore Architecture (NMDA).

--- middle

# Introduction

The flexible grid (flexi-grid) optical network technology defined by
the International Telecommunication Union Telecommunication
Standardization Sector (ITU-T) and documented in Recommendation ITU-
T_G.694.1 {{ITU-T_G.694.1}} and ITU-T_G.872 {{ITU-T_G.872}} provides an
enhanced Dense Wavelength Division Multiplexing (DWDM) grid by
defining a set of nominal central frequencies, slot widths, and the
concept of the "frequency slot".  This technology increases both
transport network scalability and flexibility, allowing the
optimization of bandwidth usage.

{{?RFC7698}} provides a framework for GMPLS-Based control of flexi-grid
DWDM networks while {{!RFC7699}} defines generalized labels for the use
of GMPLS in flexi-grid networks.

{{!RFC8363}} provides extensions to the OSPF-TE protocol so as to
support GMPLS control of flexi-grid networks.

This document presents a YANG data model {{!RFC7950}} for flexi-grid
objects in the dynamic optical network, including nodes, transponders
and links, as well as how such links interconnect nodes.  This model
is independent of control plane protocols.

This document identifies the flexi-grid components, parameters, and
their values.  It characterizes the features and the performances of
the flexi-grid elements.  For this, it augments {{!RFC8795}}, and
imports the generic Layer 0 types and use of "media-channel" defined
in {{!RFC9093}}.

An application example in {{example}} is also provided to better
understand the utility of this YANG model.

A partner document defines a second YANG module that described flexi-
grid tunnels, i.e., the paths from source to destination through a
number of intermediate nodes {{?I-D.ietf-ccamp-flexigrid-tunnel-yang}}.

Impairment-aware traffic engineering topology is described in
{{?I-D.ietf-ccamp-optical-impairment-topology-yang}}.

The YANG data model defined in this document conforms to the Network
Management Datastore Architecture (NMDA) {{!RFC8342}}.

# Terminology

Refer to {{?RFC7698}} and {{!RFC7699}} for the key terms used in this
document.

The following terms are defined in {{!RFC7950}} and are not redefined
here:

- client

- server

- augment

- data model

- data node

The following terms are defined in {{!RFC6241}} and are not redefined
here:

- configuration data

- state data

The terminology for describing YANG data models is found in
{{!RFC7950}}.

# Tree Diagram

A simplified graphical representation of the data model is used in
this document.  The meaning of the symbols in these diagrams is
defined in {{?RFC8340}}.

## Prefixes in Data Node Names

In this document, names of data nodes and other data model objects
are prefixed using the standard prefix associated with the
corresponding YANG imported modules, as shown in {{tab-prefixes}}.  It uses
prefixes from {{!RFC9093}}, {{!RFC8345}}, and {{!RFC8795}}.

| Prefix      | YANG module             | Reference       |
|-------------|--------------------------|----------------+
| l0-types    | ietf-layer0-types       | {{!RFC9093}}    |
| flexgt      | ietf-flexi-grid-topology| RFC XXXX        |
| nw          | ietf-network            | {{!RFC8345}}    |
| nt          | ietf-network-topology   | {{!RFC8345}}    |
| tet         | ietf-te-topology        | {{!RFC8795}}    |
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document.
Please remove this note.

{: #example}

# Example of Use

In order to explain how this model is used, we provide the following
example.  An optical network usually has multiple transponders,
switches (nodes) and links.  {{fig-topo}} shows a simple topology.

~~~~ ascii-art
      +----------+                                        +----------+
      |  Flexi-  |                                        | Flexi-   |
      |   grid   |                                        |  grid    |
      |  node A  |                                        | node E   |
      |          |        +------+        +------+        |          |
      |          | Link 1 |Flexi-| Link 2 |Flexi-| Link 3 |          |
      |          |<------>| grid |<------>| grid |<------>|          |
      |......... |        |node B|        |node C|        | .........|
      | Trans- : |        +------+        +------+        | : Trans- |
      | ponder : |                                        | : ponder |
      |    A   : |              +----------+              | :    E   |
      |........: |     Link 4   |Flexi-grid|   Link 5     | :........|
      |          |<------------>|   node   |<------------>|          |
      |          |              |    D     |              |          |
      |          |              +----------+              |          |
      +----------+                                        +----------+
~~~~
{: #fig-topo title="Topology Example"}

In order to configure a network media channel to interconnect
transponders A and E, first of all we have to populate the flexi-grid
topology YANG model with all elements in the network:

- We define the transponders within nodes A and E as tunnel
termination points (TTPs) and provide their internal local link
connectivity towards the node interfaces.  We also provide the
identifiers, addresses and interfaces of nodes A and B.

- We do the same for the nodes B, C and D, providing their
identifiers, addresses and interfaces, as well as the internal
connectivity matrix between interfaces.

- Then, we also define the links 1 to 5 that interconnect nodes,
indicating which flexi-grid labels are available.

- Other information, such as the slot frequency and granularity are
also provided.

# YANG Data Model for Flexi-Grid Topology

## Flexi-Grid Topology Data Model Overview

This document describes the data model for flexi-grid topology.  As a
classic traffic engineering (TE) technology, flexi-grid provides WDM
switching in transport network.  Therefore the YANG module presented
in this document augments from a more generic TE network topology
data model, i.e., the ietf-te-topology, as specified in {{!RFC8795}},
following the guidelines provided in section 6 of {{!RFC8795}}.

Common types, identities, and groupings defined in {{!RFC9093}} are
reused in this document.

The figure below shows the augmentation relationship between YANG
models.

~~~~ ascii-art
                        +-------------------------+
           TE generic   |    ietf-te-topology     |
                        +-------------------------+
                                     ^
                                     |
                                     | Augments
                                     |
                        +------------+-------------+
           Flexi-Grid   | ietf-flexi-grid-topology |
                        +--------------------------+
~~~~
{: #fig-model-hierachy title="Relationship between Flexi-Grid and TE topology models"}

The entities and TE attributes, such as node, termination points and
links, are still applicable for describing a flexi-grid topology and
the model presented in this document only specifies the technology-
specific attributes/information.

The flexi-grid specific attributes and label format is defined in
{{!RFC7699}}, including the grid type, channel spacing, slot width
granularity, n and m parameters.  A collection of common data types
have also been specified in {{!RFC9093}}, and used in this document for
augmentation of the generic TE topology model.

The YANG module ietf-flexi-grid-topology defined in this document
conforms to the Network Management Datastore Architecture (NMDA)
defined in {{!RFC8342}}.

## Augmentations for Flexi-Grid Topology and Node

There are a few characteristics augmenting to the generic TE
topology.

Following the guidelines in {{!RFC8795}}, a flexi-grid-topology network-
type is specified as the indicator of flexi-grid in the topology as
shown in {{fig-network-type}}.

~~~~ ascii-art
       augment /nw:networks/nw:network/nw:network-types/tet:te-topology:
         +--rw flexi-grid-topology!
~~~~
{: #fig-network-type title="Flexi-Grid Topology Augmentation"}

A flexi-grid-node presence container is specified, augmenting the
generic TE node attributes, to indicate that the TE node is a Flexi-
Grid node as shown in {{fig-node-type}}.

~~~~ ascii-art
          augment /nw:networks/nw:network/nw:node/tet:te
                  /tet:te-node-attributes:
            +--rw flexi-grid-node!
~~~~
{: #fig-node-type title="Flex-Grid Node Augmentation"}

It is assumed that all the flexi-grid nodes are reconfigurable.

## Bandwidth Augmentation

No bandwidth augmentations are needed for this YANG module.

As described in Section 4.2 of {{!RFC7699}}, there is some overlap
between bandwidth and label in Layer 0.

The flexi-grid label resource information described in {{flexi-label}},
is sufficient to also describe the spectrum resources within a flexi-
grid network.  Therefore, the model does not define any augmentation
for the te-bandwidth containers defined in {{!RFC8795}}.

{: #flexi-label}

## Label Augmentation

The model augments all the occurrences of the label-restriction list
in {{!RFC8795}} with flexi-grid technology specific attributes using the
flexi-grid-label-range-info grouping defined in {{!RFC9093}}.

Moreover, following the guidelines in {{!RFC8795}}, the model augments
all the occurrences of the te-label container with the flexi-grid
technology specific attributes using the flexi-grid-label-start-end,
flexi-grid-label-hop and flexi-grid-label-step groupings defined in
{{!RFC9093}}.

# YANG Model (Tree Structure) for Flexi-Grid Topology

~~~~ ascii-art
{::include ./ietf-flexi-grid-topology.tree}
~~~~
{: #fig-flexig-topo-tree artwork-name="ietf-flexi-grid-topology.tree"}

# The YANG Code for Flexi-grid topology

~~~~ yang
{::include ./ietf-flexi-grid-topology.yang}
~~~~
{: #fig-flexig-topo-yang sourcecode-markers="true" sourcecode-name="ietf-flexi-grid-topology@2023-12-15.yang"}

# Security Considerations

The YANG module specified in this document defines a schema for data
that is designed to be accessed via network management protocols such
as NETCONF {{!RFC6241}} or RESTCONF {{!RFC8040}}.  The lowest NETCONF layer
is the secure transport layer, and the mandatory-to-implement secure
transport is Secure Shell (SSH) {{!RFC6242}}.  The lowest RESTCONF layer
is HTTPS, and the mandatory-to-implement secure transport is
Transport Layer Security (TLS) {{!RFC8446}}.

The NETCONF access control model {{!RFC8341}} provides the means to
restrict access for particular NETCONF users to a preconfigured
subset of all available NETCONF protocol operations and content.  The
NETCONF Protocol SSH {{!RFC6242}} describes a method for invoking and
running NETCONF within a SSH session as an SSH subsystem.  The
Network Configuration Access Control Model (NACM) {{!RFC8341}} provides
the means to restrict access for particular NETCONF or RESTCONF users
to a preconfigured subset of all available NETCONF or RESTCONF
protocol operations and content.

A number of configuration data nodes defined in this document are
writable/deletable (i.e., "config true").  These data nodes may be
considered sensitive or vulnerable in some network environments.

There are a number of data nodes defined in this YANG module that are
writable/creatable/deletable (i.e., config true, which is the
default).  These data nodes may be considered sensitive or vulnerable
in some network environments.  Write operations (e.g., edit-config)
to these data nodes without proper protection can have a negative
effect on network operations.  These are the subtrees and data nodes
and their sensitivity/vulnerability:

~~~~ ascii-art
      /nw:networks/nw:network/nw:network-types/tet:te-topology
      /nw:networks/nw:network/nt:link/tet:te/tet:te-link-attributes
      /nw:networks/nw:network/nw:node/nt:termination-point/tet:te
      /nw:networks/nw:network/nw:node/tet:te/tet:te-node-attributes
      /te-connectivity-matrices/te-connectivity-matrix/tet:path-
      constraints/tet:te-bandwidth/tet:technology
      /nw:networks/nw:network/nw:node/tet:te
      /tet:tunnel-termination-point/tet:local-link-connectivities
      /tet:label-restrictions/tet:label-restriction
~~~~

# IANA Considerations

IANA is requested to assigned a new URI from the "IETF XML Registry"
{{!RFC3688}} as follows:

~~~~ ascii-art
         URI: urn:ietf:params:xml:ns:yang:ietf-flexi-grid-topology
         Registrant Contact: The IESG
         XML: N/A; the requested URI is an XML namespace.
~~~~

IANA is requested to assign a new YANG module name in the "YANG
Module Names" registry {{!RFC6020}} as follows:

~~~~ ascii-art
         Name: ietf-flexi-grid-topology
         Namespace: urn:ietf:params:xml:ns:yang:ietf-flexi-grid-topology
         Prefix: flexgt
         Reference: RFC XXXX
~~~~

--- back

# Acknowledgments
{:numbered="false"}

The work presented in this document has been partially funded by the
European Commission under the project H2020 METRO-HAUL (Metro High
bandwidth, 5G Application-aware optical network, with edge storage,
compUte and low Latency), Grant Agreement number: 761727.

This work is also partially funded by the Spanish State Research
Agency under the project AgileMon (AEI PID2019-104451RB-C21) and by
the Spanish Ministry of Science, Innovation and Universities under
the program for the training of university lecturers (Grant number:
FPU19/05678).

Thanks to Adrian Farrel for reviewing this document and assisting
with conversion to XML.
