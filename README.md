# Airdrop Vaults Repository

This repository contains the data used to distribute 9Summits vaults' airdrops.

## Overview

For each airdrop that a 9Summits vault is eligible for, we will distribute rewards proportionally to users based on:

- Token distribution  
- Points evolution  

## Data Sources

The distribution data used for these calculations is displayed in this repository. 

This data is computed using the \[Lagoon repository\]\(https://github.com/hopperlabsxyz/fees-computation-cli\) \(fee-computation-cli\).

## Repository Structure

The repository is organized by protocol and airdrop campaign:

- protocol
  - airdrop-campaign
    - vault-name
      - user-distribution.csv # Final airdrop allocations per user
      - raw-points.csv # Snapshot of unadjusted points data
      - interpolated-points.csv # Optional time-adjusted points (if needed)
