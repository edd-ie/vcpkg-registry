# vcpkg-registry

Vcpkg registry of private and custom ports 

## Adding Ports

For an overview of creating vcpkg ports, see [this
Microsoft blog post](https://devblogs.microsoft.com/cppblog/registries-bring-your-own-libraries-to-vcpkg/). Note that some of sample commands are
deprecated (Ex. `vcpkg_configure_cmake` has been replaced by `vcpkg_cmake_configure`).

The ports added to this project should be made such that they can be consumed by any project. They
should not provide dependencies with build options constrained to a single consuming project.
Options for development operations (building docs, unit tests, samples, examples, etc.) can be
permanently off.

Specifying build options are made by creating generic port in this a vcpkg registry (this
repository, or other), then specifying the desired features in the consuming project's [manifest
file](https://vcpkg.readthedocs.io/en/latest/specifications/manifests/). This is the proper thing to
do.

## Specific Builds

Alternatively, if generically packaging the dependency is deemed too complicated, alternative solutions within the vcpkg ecosystem are:

- [filesystem registry](https://vcpkg.io/en/docs/users/registries.html#registry-objects-path) within the consuming project
- [overlay ports](https://vcpkg.readthedocs.io/en/latest/specifications/manifests/) within the
consuming project
- A separate vcpkg registry for use only by the consuming project

Never change the name of a dependency or its CMake targets for a specific build of the dependency.
This would allow multiple instances of a dependency in a project, causing multiple definition errors
at the least, and mysterious runtime bugs at the worst. This isn't specific to vcpkg.


flowchart LR
  subgraph Client
    UI[Web app]
    Cache[(Local cache)]
  end
  subgraph Services
    API[API gateway]
    Auth[Auth service]
    Orders[Order service]
  end
  subgraph Storage
    DB[(Orders DB)]
  end
  UI --> API
  UI --> Cache
  API --> Auth
  API --> Orders
  Orders --> DB
  Auth -. token .-> UI
