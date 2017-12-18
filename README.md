# Role Name

This role sets up and configures a mattermost instance.


## Requirements

It needs an apt based system like ubuntu.


## Role Variables

[See the mattermost config for variables](https://docs.mattermost.com/administration/config-settings.html#)

| Name                     |         Required         | Default      | Description                                                              |    |    |
|:-------------------------|:------------------------:|:-------------|:-------------------------------------------------------------------------|:---|:---|
| `mattermost_version`     | :heavy_multiplication_x: | `4.5.0`      | Install version of mattermost                                            |    |    |
| `mattermost_db_user`     | :heavy_multiplication_x: | `mattermost` | Name of database user                                                    |    |    |
| `mattermost_db_password` |    :heavy_check_mark:    |              | Password for database user                                               |    |    |
| `mattermost_db_name`     | :heavy_multiplication_x: | `mattermost` | Name of database mattermost should use                                   |    |    |
| `mattermost_user`        | :heavy_multiplication_x: | `mattermost` | User under wich the mattermost process runs                              |    |    |
| `mattermost_enterprise`  | :heavy_multiplication_x: | `false`      | Select if the role should setup a enterprise version or the team version |    |    |

## Example Playbook
This will follow after testing

```yml

```
```yml

```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).


## Author Information

- [Fritz Otlinghaus (Scriptkiddi)](github profile) _fritz.otlinghaus@stuvus.uni-stuttgart.de_
