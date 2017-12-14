<span data-ttu-id="2c08d-101">Aggiungere le proprietà seguenti alla classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="2c08d-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="2c08d-102">Il campo `ID` è richiesto dal database per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="2c08d-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="2c08d-103">Aggiungere una classe di contesto dei dati</span><span class="sxs-lookup"><span data-stu-id="2c08d-103">Add a database context class</span></span>

<span data-ttu-id="2c08d-104">Aggiungere una classe derivata `DbContext` denominata *MovieContext.cs* alla cartella *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="2c08d-104">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>
[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

<span data-ttu-id="2c08d-105">Il codice precedente crea una proprietà `DbSet` per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="2c08d-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="2c08d-106">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella.</span><span class="sxs-lookup"><span data-stu-id="2c08d-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>