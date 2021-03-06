---
description: |
    The linode Packer builder is able to create new images for use with Linode. The
    builder takes a source image, runs any provisioning necessary on the image
    after launching it, then snapshots it into a reusable image. This reusable
    image can then be used as the foundation of new servers that are launched
    within all Linode regions.
layout: docs
page_title: 'Linode - Builders'
sidebar_current: 'docs-builders-linode'
---

# Linode Builder

Type: `linode`

The `linode` Packer builder is able to create [Linode
Images](https://www.linode.com/docs/platform/disk-images/linode-images/) for
use with [Linode](https://www.linode.com). The builder takes a source image,
runs any provisioning necessary on the image after launching it, then snapshots
it into a reusable image. This reusable image can then be used as the
foundation of new servers that are launched within Linode.

The builder does *not* manage images. Once it creates an image, it is up to you
to use it or delete it.

## Configuration Reference

There are many configuration options available for the builder. They are
segmented below into two categories: required and optional parameters. Within
each category, the available configuration keys are alphabetized.

In addition to the options listed here, a
[communicator](/docs/templates/communicator.html) can be configured for this
builder.

### Required

-   `linode_token` (string) - The client TOKEN to use to access your account.

-   `image` (string) - An Image ID to deploy the Disk from. Official Linode
    Images start with `linode/`, while user Images start with `private/`. See
    [images](https://api.linode.com/v4/images) for more information on the
    Images available for use. Examples are `linode/debian9`, `linode/fedora28`,
    `linode/ubuntu18.04`, `linode/arch`, and `private/12345`.

-   `region` (string) - The id of the region to launch the Linode instance in.
    Images are available in all regions, but there will be less delay when
    deploying from the region where the image was taken. Examples are
    `us-east`, `us-central`, `us-west`, `ap-south`, `ca-east`, `ap-northeast`,
    `eu-central`, and `eu-west`.

-   `instance_type` (string) - The Linode type defines the pricing, CPU, disk,
    and RAM specs of the instance. Examples are `g6-nanode-1`, `g6-standard-2`,
    `g6-highmem-16`, and `g6-dedicated-16`.

### Optional

-   `instance_label` (string) - The name assigned to the Linode Instance.

-   `instance_tags` (list) - Tags to apply to the instance when it is created.

-   `swap_size` (int) - The disk size (MiB) allocated for swap space.

-   `image_label` (string) - The name of the resulting image that will appear
    in your account. Defaults to "packer-{{timestamp}}" (see [configuration
    templates](/docs/templates/engine.html) for more info).

-   `image_description` (string) - The description of the resulting image that
    will appear in your account. Defaults to "".

-   `state_timeout` (string) - The time to wait, as a duration string, for the
    Linode instance to enter a desired state (such as "running") before timing
    out. The default state timeout is "5m".

## Basic Example

Here is a Linode builder example. The `linode_token` should be replaced with an
actual [Linode Personal Access
Token](https://www.linode.com/docs/platform/api/getting-started-with-the-linode-api/#get-an-access-token).

```json
{
  "type": "linode",
  "linode_token": "YOUR API TOKEN",
  "image": "linode/debian9",
  "region": "us-east",
  "instance_type": "g6-nanode-1",
  "instance_label": "temporary-linode-{{timestamp}}",

  "image_label": "private-image-{{timestamp}}",
  "image_description": "My Private Image",

  "ssh_username": "root"
}
```
