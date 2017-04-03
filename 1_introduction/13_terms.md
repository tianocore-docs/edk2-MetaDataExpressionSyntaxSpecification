<!--- @file
  1.3 Terms

  Copyright (c) 2014-2017, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

## 1.3 Terms

The following terms are used throughout this document to describe varying
aspects of input localization:

**BaseTools**

The BaseTools are the tools required for an EDK II build.

**BDS**

Boot Devices Selection phase.

**BNF**

BNF is an acronym for "Backus Naur Form". John Backus and Peter Naur introduced
for the first time a formal notation to describe the syntax of a given language.

**Component**

An executable image. Components defined in this specification support one of
the defined module types.

**DEC**

EDK II Package Declaration File. This file declares information about what is
provided in the package. An EDK II package is a collection of like content.

**DEPEX**

Module dependency expressions that describe runtime process restrictions.

**Dist**

This refers to a distribution package that conforms to the UEFI Platform
Initialization Distribution Package Specification.

**DSC**

EDK II Platform Description File. This file describes what and how modules,
libraries and components are to be built, as well as defining library instances
which will be used when linking EDK II modules.

**DXE**

Driver Execution Environment.

**DXE SAL**

A special class of DXE modules that provides SAL Runtime Services. DXE SAL
modules differ from DXE Runtime modules in that the DXE Runtime modules support
Virtual mode OS calls at OS runtime and DXE SAL modules support intermixing
Virtual or Physical mode OS calls.

**DXE SMM**

A special class of DXE modules that are loaded into the System Management Mode
memory.

**DXE Runtime**

A special class of DXE modules that provide Runtime Services.

**EBNF**

Extended "Backus Naur Form" meta-syntax notation with the following additional
constructs: square brackets "[...]" surround optional items, suffix "*" for a
sequence of zero or more of an item, suffix "+" for one or more of an items,
suffix "?" for zero or one of an item, curly braches "{...}" enclosing a choice
of a list of alternatives and superscripts indicating between n and m
occurrences.

**EDK**

Extensible Firmware Interface Development Kit, the original implementation of
the Intel(R) Platform Innovation Framework for EFI Specifications developed in
2007.

**EDK II**

EFI Development Kit, version II that provides updated firmware module layouts
and custom tools, superseding the original EDK.

**EDK Compatibility Package (ECP)**

The EDK Compatibility Package (ECP) provides libraries that will permit using
most existing EDK drivers with the EDK II build environment and EDK II
platforms.

**EFI**

Generic term that refers to one of the version of the EFI or UEFI
specifications.

**FDF**

EDK II Flash Definition File. This file is used to define the content and
binary image layouts for firmware images, update capsules and PCI option ROMs.

**FLASH**

This term is used throughout this document to describe one of the following:

* An image that is loaded into a hardware device on a platform - traditional
  ROM image.

* An image that is loaded into an Option ROM device on an add-in ard

* A bootable image that is installed on removable, bootable media, such as a
  Floppy, CD-ROM or USB storage device.

* A UEFI application that can be accessed during but (at an EFI Shell prompt),
  prior to hand-off to the OS loader.

**Foundation**

The set of code and interfaces that holds implementations of EFI together.

**Framework**

Intel(R) Platform Innovation Framework for EFI consist of the Foundation, plus
other modular components that characterize the portability surface for modular
components designed to work on any implementation of the Tiano architecture.

**GUID**

Globally Unique Identifier. A 128-bit value used to name entities uniquely. A
unique GUID can be generated by an individual without the help of a centralized
authority. This allows the generation of names that will never conflict, even
among multiple, unrelated parties. GUID values can be registry format,
8-4-4-4-12, or C data structure format.

GUID also refers to an API named by a GUID.

**HII**

Human Interface Infrastructure. This generally refers to the database that
contains string, font and IFR information along with other pieces that use one
of the database components.

**HOB**

Hand-off blocks are key architectural mechanisms that are used to hand off
system information in the early pre-boot phase.

**IFR**

Internal Forms Representation. This is the binary encoding that is used for the
representation of user interface pages.

**INF**

EDK II Module Information File. This file describes how the module is coded.

**Library Class**

**A library class defines the API or interface set for a library. The consumer
of the library is coded to the library class definition. Library classes are
defined via a library class .h file that is published by a package.**

**Library Instance**

An implementation of one or more library classes.

**Module**

A module is either an executable image or a library instance. For a list of
module types supported by a package, see Module Type.

**Module Type**

All libraries and components belong to one of the following module types:
`BASE`, `SEC`, `PEI_CORE`, `PEIM`, `DXE_CORE`, `DXE_DRIVER`,
`DXE_RUNTIME_DRIVER`, `DXE_SMM_DRIVER`, `DXE_SAL_DRIVER`, `UEFI_DRIVER` or
`UEFI_APPLICATION`. These definitions provide a framework that is consistent
with a similar set of requirements. A module that is of type module type `BASE`,
depends only on header and libraries provided in the MdePkg, while a module
that is of module type `DXE_DRIVER` depends on common DXE components. An
additional module type, `USER_DEFINED`, is allowed for extensibility. The EDK II
build system also permits modules of type `USER_DEFINED`. These modules will not
be processed by the EDK II Build system.

**Numeric Values**

Numeric values in the EDK II meta-data file expressions are either unsigned
integer values (base 10) or hexadecimal values (base 16). No other numeric data
types are permitted.

**Package**

A package is a container. It can hold a collection of files for any given set
of modules. Packages may be described as containing zero or more of any of the
following:

* Source modules, containing all source files and descriptions of a module.

* Binary modules, containing EFI sections for a Framework File System and a
  description file specific to linking and binary editing of features and
  attributes specified in a Platform Configuration Database (PCD).

* Mixed modules, with both binary and source modules.

Multiple modules can be combined into a package and multiple packages can be
combined into a single package.

**PCD**

Platform Configuration Database.

**PEI**

Pre-EFI Initialization Phase

**PEIM**

An API named by GUID.

**PI**

UEFI Platform Initialization Specification.

**PPI**

A PEIM-to-PEIM interface that is named by a GUID.

**Protocol**

An API named by GUID.

**Runtime Services**

Interfaces that provide access to underlying platform-specific hardware that
might be useful during OS runtime, such as time and date services.

These services become active during the boot process but also persist after the
OS loader terminates boot services.

**SAL**

System Abstraction Layer. A firmware interface specification used on Intel(R)
Itanium(R) Processor based systems.

**SEC**

Security Phase is the code in the Framework that contains the processor reset
vector and launches PEI. This phase is separate from PEI because some security
schemes require ownership of the reset vector.

**SKU**

Stock Keeping Unit.

**SMM**

System Management Mode. A generic term for the execution mode entered when a
CPU detect an SMI. The firmware, in response to the interrupt type, will gain
control in physical mode. For this document, "SMM" describes the operational
regime for IA32 and x64 processors that share the OS-transparent
characteristics.

**UEFI**

Unified Extensible Firmware Interface

**UEFI Application**

An application that follows the UEFI Specification. The only difference between
a UEFI application and a UEFI driver is that an application is unloaded from
memory when it exits regardless of return status, while a driver that returns a
successful return status is not unloaded when its entry point exits.

**UEFI Driver**

A driver that follows the UEFI Specification.

**UEFI Driver**

A driver that follows the UEFI specification.

**UEFI Specification Version 2.4**

Current UEFI version.

**UEFI Platform Initialization Distribution Package Specification 1.0**

The current version of this specification includes Errata B.

**UEFI Platform Initialization Specification 1.3**

Current version of the UEFI PI Specification.

**Unified EFI Forum**

A non-profit collaborative trade organization formed to promote and manage the
UEFI standard. For more information, see http://www.uefi.org

**VFR**

Visual Forms Representation.

**VPD**

Vital Product Data that is read-only binary configuration data, typically
located within a region of a flash part. This data would typically be updated
as part of a firmware build, post firmware build (via patching tools), through
automation on a manufacturing line as the 'FLASH' parts are programmed or
through special tools.
