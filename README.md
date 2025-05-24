# SecureSessionVault (.NET)

A plug-and-play .NET package for securely storing and retrieving session data via HTTP using `efetch` and `ethernmyth/secure-session-vault` from docker. Requires zero boilerplate in consuming apps.

## Features

- Static access: `SecureSession.GetItem("key")`
- Uses `efetch` to persist session to remote API
- Configurable via `appsettings.json`

## Important Notes

- This package is designed to work with the `ethernmyth/secure-session-vault` Docker image. You must have this image running to use the package.
- Follow the instructions below to set up the Docker image and container.

- Run the backend using Docker, the backend is available on docker hub at[Docker Hub](https://hub.docker.com/r/ethernmyth/secure-session-vault):

```bash
docker run -p 17000:17000 ethernmyth/secure-session-vault:latest
```

Or include it in your Docker Compose setup with:

```yaml
services:
  vault:
    image: ethernmyth/secure-session-vault:latest
    ports:
      - "17000:17000"
```

## Configuration

```json
{
    "Efetch": {
        "BaseUrl": "http://localhost:17000",
        "DefaultHeaders": {
          "Authorization": null,
          "Accept": "application/json"
        },
        "RetryCount": 3
    }
}
```

## Usage

```csharp
Program.cs
-----------------

using SecureSessionVault.Extensions;

builder.Services.AddSecureSessionVault(builder.Configuration);

SampleController.cs
------------------

using Microsoft.AspNetCore.Mvc;
using SecureSessionVault.Static;

namespace SecureSessionVault.Test.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class SampleController : ControllerBase
    {
        [HttpGet("{key}")]
        public async Task<IActionResult> Get(string key)
        {
            var value = await SecureSession.GetItem(key);
            return Ok(value);
        }

        [HttpPost("{key}/{value}")]
        public async Task<IActionResult> Post(string key, string value)
        {
            var results = await SecureSession.SetItem(key, value);
            return Ok(results);
        }

        [HttpDelete("{key}")]
        public async Task<IActionResult> Delete(string key)
        {
            var results = await SecureSession.DeleteItem(key);
            return Ok(results);
        }
    }
}

```

## Author

Created and Maintained by: [Ethern-Myth](https://github.com/ethern-myth)

---

**Give a like to this project, Thanks.**

