# Discobox Homebrew tap

```sh
brew install discobox-ai/tap/discobox
```

macOS on Apple Silicon, and Linux on x86-64 or arm64.

Intel Macs are not served. ADR 0062 defers them and `assemble-guest-image`
refuses non-arm64, so an Intel binary would install and then fail at the first
pool — which is a worse outcome than not offering one.

## The formula is generated

`Formula/discobox.rb` is written by `task brew:formula` in
[discobox-ai/discobox](https://github.com/discobox-ai/discobox) from the assets
a release actually uploaded, so its checksums cannot drift from what a user
downloads. Edit that target, not the formula.

## macOS entitlements

The darwin binary is ad-hoc signed with `com.apple.security.virtualization`,
which Virtualization.framework requires to create a VM. Signing happens at
build time, never here: the entitlement lives in the Mach-O's signature blob,
`install` preserves it, and re-signing in the formula would mutate a binary
Homebrew had just verified.

`com.apple.vm.networking` is deliberately absent — it is what would put pool
guests on your LAN. They are NAT'd by the framework instead.
