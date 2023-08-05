# TurtleShell

A Solidity library for DeFi protocols to help protect against exploits.

## How TurtleShell works

For a more detailed explanation of TurtleShell, please refer to our official [docs](https://turtleshell.gitbook.io/introduction/).

The on-chain Firewall works by allowing protocols to specify a security parameter (number) that may not change by more than a given amount, in a given time window. Using the TVL as the security parameter would effectively enable protocols to track huge and sudden TVL fluctuations, which are highly indicative of malicious activity, directly on-chain.

Protocols may also choose to store multiple security parameters, to track multiple metrics at the same time. For example, a protocol may choose to track both the TVL and the number of users, to protect against both flash loan attacks and rug pulls.

Protocol’s may choose to use or multiple security parameter(s). Each security parameter is referenced by its creator (an address) and a unique identifier. These identifiers may be a token address, a hash, or any arbitrary Solidity bytes32 value. Said approach maximizes compatibility, by practically allowing protocol’s to track metrics for specific ERC20 tokens or other unique identifiers separately.

In Solidity, the security parameter value is stored using the following data structure:

```solidity
/// @dev owner address => security parameter identifier => security parameter value
mapping (address => mapping(bytes => uint256)) s_securityParameters;
```

### Configuration

The Firewall configuration can be set up by the protocol governance / owner, by defining a set of rules for the individual Firewall implementation. This is done by calling the `setUserConfig` function in the TurtleShell contract. The `setUserConfig` function accepts the following arguments:

- `uint256 startParameter`: The initial value of the security parameter
- `uint8 thresholdPercentage`: The maximum allowed change of the security parameter, in a given time window, in percentage points (between 0-100)
- `uint256 blockInterval`: The time window in which the security parameter may not change by more than the threshold percentage
- `uint256 cooldownPeriod`: Optional cooldown period in blocks, after which the Firewall automatically de-activates in case of being triggered. Set this value to `type(uint256).max` in case you do not want to use a cooldown period.

The combination of these rules allow for highly individualistic and dynamic Firewall implementations.

It is very important to make sure to set these rules adequately for your needs. The efficiency of the Firewall implementation heavily relies on good configuration values, to ensure high efficiency and a low false-positives rate.

### Integration

TurtleShell is designed to be easily integrated into existing DeFi protocols. The following steps are required to integrate TurtleShell into your protocol:

- Define configuration values for the Firewall: The first step is to define the parameters for your Firewall. This includes the starting parameter, the time-period, and the threshold. The choice of parameters will depend on what you want to track and the specific needs of your protocol.
- Integrate TurtleShell into your protocol: Once the configuration values have been set, protocol’s can implement the on-chain Firewall. This includes code manipulation within the protocol’s codebase.
- Monitor and adjust: After integration, protocol’s can monitor the operation of the Firewall and adjust the configuration values as needed. This allows for fine-tuning the system to better protect the protocol.

Protocols have to ensure that the security parameter of the Firewall is updated, whenever it needs to. This means including calling the `updateSecurityParameter` (or `increaseParameter` / `decreaseParameter`) function in the protocol's codebase, whenever the security parameter needs to be updated.

### Contracts

For a detailed documentation of the Solidity Smart Contract comprising TurtleShell, please refer to the Smart Contract docs [here](./docs/TurtleShellFirewall.md).

### Demo Example

You can find an example integration of TurtleShell inside of the `contracts/demo` folder. This example consists of a minimalistic DeFi protocol, which implements TurtleShell to protect itself. The demo protocol is designed to be exploitable, by allowing anyone to call the `adminEmergencyWithdraw` function, which is intended to only be called by the admin of the protocol.

This example showcases, that, even if protocol's have massive security flaws, TurtleShell will still be able to protect and prevent these grave flaws from being taken advantage of.

### Extensions

This repository does not include various extension to the core Firewall module. These extensions are highly useful, to automatically implement protective measures once the Firewall gets triggered.

Protective measures may include, but are not limited to:

- Blocking (the) transaction(s) causing the security parameter threshold to be exceeded
- Freezing affected funds
- Throttle fund withdrawal
- Pausing the entire protocol
