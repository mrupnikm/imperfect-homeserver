# (Im)perfect Homeserver

This is a respository containing a simple selfhosted setup for my home server instance running docker on a Raspberry PI 4 with an external SSD for storage

## Architecture

The main premise is to have a dockerised evironment where the volumes are stored on the external drve of the Pi. The file system on the SSD is defined as BTRFS to make use of its snapshoting

The device is mounted in this case in `/mnt/main` directory with where there is a root directory of `data` and the snapshots for it are stored in `data/.snapshots` per application name. 

Example of data directory:

```
.
└── data
    ├── immich
    │   └── ...
    ├── nextcloud
    │   └── ...
    ├── paperless
    │   └── ...
    ├── photoprism
    │   └── ...
    ├── .snapshots
    │   ├── immich
    │   ├── nextcloud
    │   ├── photoprism
    │   └── trilium
    └── trilium
        └── ...


```

Example of snapshots directory:

```
.snapshots/
├── immich
│   ├── 2023-06-30T11:38:13Z
│   └── 2023-08-09T11:45:24Z
├── nextcloud
│   ├── 2023-06-30T08:07:38Z
│   ├── 2023-06-30T11:36:41Z
│   ├── 2023-06-30T11:38:13Z
│   └── 2023-08-09T11:45:24Z
├── photoprism
│   ├── 2023-06-30T11:36:41Z
│   ├── 2023-06-30T11:38:13Z
│   └── 2023-08-09T11:45:24Z
└── trilium
    ├── 2023-06-30T11:36:41Z
    ├── 2023-06-30T11:38:13Z
    └── 2023-08-09T11:45:24Z

```

## Snapshoting

in the `/snapshoting` there are 2 main playbooks:
- snapshot.yaml: to snapshot the desired directories into .snapshot folder after the docker process has been stopped
- resotore.yaml: to restore the n-th created snapshot based on creation time

