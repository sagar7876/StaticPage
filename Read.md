
## File: `MauiApp1/MauiApp1/Platforms/Android/network_security_config.xml`

```plaintext
﻿<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
	<domain-config cleartextTrafficPermitted="true">
		<domain includeSubdomains="true">www.ibm.com</domain>
	</domain-config>
</network-security-config>
```

## File: `MauiApp1/MauiApp1/Platforms/MacCatalyst/AppDelegate.cs`

```csharp
﻿using Foundation;

namespace MauiApp1

    .blazor-error-boundary::after {
        content: "An error has occurred."
    }

.status-bar-safe-area {
    display: none;
}

@supports (-webkit-touch-callout: none) {
    .status-bar-safe-area {
        display: flex;
        position: sticky;
        top: 0;
        height: env(safe-area-inset-top);
        background-color: #f7f7f7;
        width: 100%;
        z-index: 1;
    }
.flex-column, .navbar-brand {
        padding-left: env(safe-area-inset-left);
    }
}
