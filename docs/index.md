---
slug: /
sidebar_position: 1
---

# Introduction

## Background

The government contracting model and the sheer size of the VA mean hundreds of teams have created or are creating APIs for the organization. As methodologies, architectures, and documentation standards vary from team to team, it's unsurprising that in the past the department provided fractured and inconsistent APIs to its consumers.

## Purpose

The focus of the standards are the 'I' in API, the interface. It is not an application design guide or a programming manual. How teams develop an API behind that interface is up to them. The guide's main goal is to outline common interface and documentation standards for APIs deployed in Lighthouse.

We do not intend for this guide to be read back-to-front. That said, the first section, [General Guidelines](general-guidelines), applies to all APIs on Lighthouse, and implementing teams should read it in its entirety. 

The remaining sections are more of a field manual or cookbook that is skimmed initially. Later, when presented with a problem whose solution is ambiguous, teams can reference this guide for Lighthouse's recommended solution or design pattern.

## Conventions

Some items in the API standards are hard and fast rules, other are recommendations.
[RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) describes the keywords which signify the requirements in this guide. Those keywords appear in **bold** within their respective highlighted admonitions as below:

!!! warning "Requirement"
    - Things you **must** and **must not** do as an absolute requirement of being on Lighthouse.

!!! success "Guidance"
    - API best practices that are **recommended** and those that are **not recommended**.

!!! info
    - Additional information and tips.
