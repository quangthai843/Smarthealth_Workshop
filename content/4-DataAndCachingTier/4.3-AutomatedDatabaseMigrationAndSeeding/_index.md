---
title: "Automated Database Migration & Seeding"
weight: 3
chapter: false
pre: " <b> 4.3 </b> "
---

In a true cloud-native environment, you should never manually execute SQL scripts or click through a graphical database tool (like pgAdmin) to build your production schema.

Because we are deploying ASP.NET Core 9 inside isolated containers, we will use Entity Framework (EF) Core to automatically construct the tables and seed administrative data the moment the container spins up.

### Updating Program.cs for Automated Initialization

Ensure that your Program.cs file includes the `MigrateAsync()` command explicitly placed before your database seeder logic. This guarantees the blank RDS database is formatted correctly before any INSERT queries run.

```csharp
using Microsoft.EntityFrameworkCore; // Ensure this is at the top

// ... (builder and app setup)

using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();

    // 1. Automatically creates tables in the empty RDS database!
    await db.Database.MigrateAsync();

    // 2. Safely injects the default Admin accounts and Demo Doctors
    await DbSeeder.SeedAsync(db);

    var useLocal = builder.Configuration.GetValue("Aws:UseLocalEmulators", true);
    if (useLocal)
    {
        // ... (Local MinIO bucket creation logic)
    }
}
```

![MigrateAsync method inside Program.cs](/images/rds/migrate-async-code.png)