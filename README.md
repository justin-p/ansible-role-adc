# ansible-role-adc

[![Ansible Role Name](https://img.shields.io/ansible/role/d/justin_p/adc?style=flat-square
)](https://galaxy.ansible.com/justin_p/adc)
[![Github Actions](https://img.shields.io/github/actions/workflow/status/justin-p/ansible-role-adc/main.yml?label=Github%20Actions&logo=github&style=flat-square)](https://github.com/justin-p/ansible-role-adc/actions)

This role will add a addtional Domain Controller to a existing Active Directory Domain/Forest. No hardening is applied.

Based of the work done by [@jborean93](https://github.com/jborean93) in [jborean93/ansible-windows](https://github.com/jborean93/ansible-windows)

Works on

- Windows Server 2019

Not tested on

- Windows Server 2016
- Windows Server 2012R2

## Requirements

- `python3-winrm` (`pywinrm`) is needed for WinRM.

## Role Variables

`defaults/main.yml`

| Variable                      | Description                                                                                                                                                      | Default value                                                                            |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| adc_administrator_username    | Setting this to Built-in Administrator account ensure that we know the password of NETBIOS\Administrator. 9/10 times you should leave this to the default value. | Administrator                                                                            |
| adc_administrator_password    | The password that will be used to connect to AD.                                                                                                                 | P@ssw0rd!                                                                                |
| adc_dns_nics                  | The name of the ethernet adapter to setup DNS on. Defaults to wildcard. 9/10 times you should leave this to the default value.                                   | *                                                                                        |
| adc_dns_servers               | The DNS server to use on adc_dns_nics. Defaults to `{{ ansible_host }}`. You should change this to ["ip.of.pdc", "127.0.0.1"].                                   | {{ ansible_host }}                                                                       |
| adc_domain                    | The domain to join                                                                                                                                               | ad.example.test                                                                          |
| adc_domain_safe_mode_password | The DSRM password of this DC                                                                                                                                     | P@ssw0rd!                                                                                |
| adc_required_psmodules        | PowerShell/DSC modules to install from the PSGallery.                                                                                                            | [xPSDesiredStateConfiguration, NetworkingDsc, ComputerManagementDsc, ActiveDirectoryDsc] |
| adc_required_features         | Windows Features that should be installed on the Domain Controller.                                                                                              | ["AD-domain-services", "DNS"]                                                            |
| adc_desired_dns_forwarders    | The desired DNS Forwarders for the DC                                                                                                                            | ["8.8.8.8", "8.8.4.4"]                                                                   |
| adc_state                     | The state of the DC                                                                                                                                              | domain_controller                                                                        |
| adc_install_dns               | If DNS should be installed                                                                                                                                       | yes                                                                                      |
| adc_read_only                 | If this DC should be a RODC                                                                                                                                      | no                                                                                       |
| adc_database_path             | Install location of the AD database                                                                                                                              | "%SYSTEMROOT%\\NTDS"                                                                     |
| adc_domain_log_path           | Install location of the AD database logs                                                                                                                         | "%SYSTEMROOT%\\NTDS"                                                                     |
| adc_sysvol_path               | Install location of the AD SYSVOL                                                                                                                                | "%SYSTEMROOT%\\SYSVOL"                                                                   |

## Dependencies

- WinRM on the windows host should configured for Ansible.
- justin_p.posh5
- justin_p.wincom

## Example Playbook

    - hosts: secondary_domain_controller
      roles:
        - role: justin_p.posh5
        - role: justin_p.wincom
        - role: justin_p.adc

See https://github.com/justin-p/ansible-role-adc/blob/master/tests/inventory.yml for an example inventory.

## Local Development

This role includes a Vagrantfile that will spin up a local Windows Server 2019 VM in Virtualbox.  
After creating the VM it will automatically run our role.

### Development requirements

`pip3 install pywinrm`

#### Usage

- Run `vagrant up` to create a VM and run our role.
- Run `vagrant provision` to reapply our role.
- Run `vagrant destroy -f && vagrant up` to recreate the VM and run our role.
- Run `vagrant destroy` to remove the VM.

## License

MIT

## Authors

- Justin Perdok ([@justin-p](https://github.com/justin-p/)), Orange Cyberdefense

## Contributing

Feel free to open issues, contribute and submit your Pull Requests. You can also ping me on Twitter ([@JustinPerdok](https://twitter.com/JustinPerdok)).
