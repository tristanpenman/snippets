# Convert Hex to Base64

## Problem

Say we have a hex-encoded string, such as the following:

    88a6d54f56f3688b56ed9c3810b53c80604f37910bc3ded297008508cad3e097ed8ea54bfd5a9c364d79aad652a6f08ff069781a5b427c1dac64ffb685dbaaaaa24316ee5d1c5efbcc9d8b17688fa100a40874459a20379063f44f2058747e5f66d5b08e3baf2120b04965a6460ad6232a453475578db7047c1a8b779a4ee27d21b6b05f73cb0497aadcc08894429efb2bbec09e31cc2a163c965ead5f52336ac05dace097b459b320cf713123316e95f521a2d72f136e42f3893c92331b98aa68ff6afe7f0573e000697921b1611018d25ed41efff564a8efcbbf61617cc91f11fd721b4f683cb755ef443f781064d4a895623b56028eaa7e4d90140f0f473f

Assuming this is the output of another program (such as a signing tool), how can we easily convert it to Base64 in a Bash script?

## Solution

We can chain `xxd` and `base64`. e.g:

    signing_tool | xxd -r -p | base64

Passing `-r` to `xxd` tells it to read a hexdump, and `-p` means that the dump is a continuous hex string. The output will be binary.

The raw binary output is then fed to `base64`. For longer strings, it may also be necessary to pass `-w 0` to `base64`, to ensure that the output is not broken across multiple lines. e.g.:

    signing_tool | xxd -r -p | base64 -w 0

## Reference

* https://superuser.com/questions/158142/how-can-i-convert-from-hex-to-base64