# Vagrant Box for a Fedora Based Suricata IPS with AF_PACKET

## Setup

    vagrant up

## Running Suricata

    suricata -c /vagrant/suricata.yaml --af-packet

## Protecting a VM

Create another VM using internal network, on the same internal network
as the second adapter on the IPS VM, likely "intnet".
