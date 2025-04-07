---
title: "Use VSCodium with Microsoft's proprietary marketplace"
source: "https://www.flypenguin.de/2023/02/26/use-vscodium-with-microsofts-proprietary-marketplace/"
author:
published: 2023-02-26
created: 2025-04-07
description: "I’d rather use VSCodium than any official MS product, unfortunately they blocked it from the official store. Or … tried to, at least."
tags:
  - "clippings"
---
<!-- markdownlint-disable MD025 -->

# Use VSCodium with Microsoft's proprietary marketplace

Posted on February 26, 2023 (Last modified on July 2, 2024) • 1 min read • 198 words Share via

Yeah, MicroSoft again. The Company That Embraced Open Source (as long as it’s useful to them).

The original VS Code builds have telemetry. Also, a closed source marketplace. Because of that, VSCodium was created. And VSCodium is good.

Unfortunately, MicroSoft is still anything but a “good” company, and it does open source purely out of egoistic motives. So it developed a (really good) open source editor (VS Code), but with closed source extensions (Pylance), which are only available on *their* marketplace, and don’t run on alternative builds of VS Code. Because, hey, more lock-in, more good, ya know?

So if you want to run Pylance (a good language server) with VS Codium, you have to switch VS Codium back to the original market place, and modify the editor’s “name” identification.

Here’s how to do that:

- on a Mac, create the file `product.json` in the folder `$HOME/Library/Application Support/VSCodium`
- enter the following content:

 ```json
 {
   "nameShort": "Visual Studio Code",
   "nameLong": "Visual Studio Code",
   "extensionsGallery": {
     "serviceUrl": "https://marketplace.visualstudio.com/_apis/public/gallery",
     "cacheUrl": "https://vscode.blob.core.windows.net/gallery/index",
     "itemUrl": "https://marketplace.visualstudio.com/items"
   }
 }
 ```

Then you cann install Pylance from the official marketplace, and it should run.

Sources:

- [VSCodium docs](https://www.flypenguin.de/github.com/VSCodium/vscodium/blob/master/DOCS.md#how-to-use-a-different-extension-gallery)
- [Some reddit thread](https://www.flypenguin.de/www.reddit.com/r/linux/comments/k0s8qw/comment/ggnqes7)
- [Some github issue entry](https://www.flypenguin.de/github.com/flathub/com.vscodium.codium/issues/90)
- [Some VSCodium github issue comment](https://www.flypenguin.de/github.com/VSCodium/vscodium/issues/892#issuecomment-986663776)
[Terraform for\_each and output values](https://www.flypenguin.de/2023/03/14/terraform-for-each-and-output-values/) [K8S Cluster autoscaler crashlooping on EKS](https://www.flypenguin.de/2023/02/23/k8s-cluster-autoscaler-crashlooping-on-eks/)
