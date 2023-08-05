# ITurtleShellFirewallIncreaser



> ITurtleShellFirewallIncreaser - Extension for the TurtleShell Firewall contract

This interface extension includes methods for automatically increasing and decreasing the parameters for a given protocol



## Methods

### decreaseParameter

```solidity
function decreaseParameter(uint256 amount) external nonpayable returns (bool)
```

Decrease the parameter for the calling protocol by a given amount



#### Parameters

| Name | Type | Description |
|---|---|---|
| amount | uint256 | The amount to decrease the parameter by |

#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | bool | undefined |

### getFirewallStatusOf

```solidity
function getFirewallStatusOf(address user) external view returns (bool)
```

Function for getting the firewall status for a given user



#### Parameters

| Name | Type | Description |
|---|---|---|
| user | address | The address to get the firewall status for |

#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | bool | bool if the firewall is active for the given user |

### getParameterOf

```solidity
function getParameterOf(address user) external view returns (uint256)
```

Function for getting the security parameter for a given firewall user



#### Parameters

| Name | Type | Description |
|---|---|---|
| user | address | The address of the firewall user |

#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | uint256 | uint256 the security parameter for the given user |

### getSecurityParameterConfigOf

```solidity
function getSecurityParameterConfigOf(address user) external view returns (uint8, uint256)
```

Function for getting the security parameters for a given address



#### Parameters

| Name | Type | Description |
|---|---|---|
| user | address | The address to get the security parameters for |

#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | uint8 | Returns The threshold and block interval set as security parameters for the address |
| _1 | uint256 | undefined |

### increaseParameter

```solidity
function increaseParameter(uint256 amount) external nonpayable returns (bool)
```

Increase the parameter for the calling protocol by a given amount



#### Parameters

| Name | Type | Description |
|---|---|---|
| amount | uint256 | The amount to increase the parameter by |

#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | bool | undefined |

### setFirewallStatus

```solidity
function setFirewallStatus(bool newStatus) external nonpayable
```

Function for manually setting the firewall status for a given user

*This function can be used to manually activate or deactivate the firewall for a given user ATTENTION: This function should especially be used to deactivate the firewall, in case it got triggered. This function emits the {FirewallStatusUpdate} event*

#### Parameters

| Name | Type | Description |
|---|---|---|
| newStatus | bool | The new status to set for the firewall |

### setParameter

```solidity
function setParameter(uint256 newParameter) external nonpayable returns (bool)
```

Function for updating the security parameter

*This function can be called by any user to update their security parameter. If the parameter exceeds the threshold, the firewall will be automatically activated. If the firewall is already active, the parameter will be updated anyways. Emits the {ParameterChanged} event Emits the {FirewallStatusUpdate} event*

#### Parameters

| Name | Type | Description |
|---|---|---|
| newParameter | uint256 | is the new parameter |

#### Returns

| Name | Type | Description |
|---|---|---|
| _0 | bool | Returns true if the firewall was activated, or had alrady been active |

### setUserConfig

```solidity
function setUserConfig(uint8 thresholdPercentage, uint256 blockInterval, uint256 startParameter, uint256 cooldownPeriod) external nonpayable
```

Function for setting the configuration values for a firewall user

*The function emits the {ParameterChanged} event*

#### Parameters

| Name | Type | Description |
|---|---|---|
| thresholdPercentage | uint8 | The threshold percentage to set for the firewall |
| blockInterval | uint256 | The block interval to set for the firewall |
| startParameter | uint256 | undefined |
| cooldownPeriod | uint256 | undefined |



## Events

### FirewallStatusUpdate

```solidity
event FirewallStatusUpdate(address indexed user, bool indexed newStatus)
```

Event emitted whenever the firewall status for a given user gets changed



#### Parameters

| Name | Type | Description |
|---|---|---|
| user `indexed` | address | undefined |
| newStatus `indexed` | bool | undefined |

### ParameterChanged

```solidity
event ParameterChanged(address indexed user, uint256 indexed newParameter)
```

Event emitted whenever the parameter for a given user gets changed



#### Parameters

| Name | Type | Description |
|---|---|---|
| user `indexed` | address | undefined |
| newParameter `indexed` | uint256 | undefined |



