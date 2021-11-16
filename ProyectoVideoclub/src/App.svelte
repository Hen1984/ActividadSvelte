<script>
    import { db } from "./firebase";
    import {
        collection,
        getDocs,
        doc,
        addDoc,
        updateDoc,
        deleteDoc,
    } from "firebase/firestore";

    let film = {
        title: "",
        year: "",
        category: "",
        poster: "",
    };
    function rentFilm () {
        alert('Rented Movie')
    };
    //FUNCIONES PELICULaS
    let films = [];

    const loadFilm = async () => {
        const querySnapshot = await getDocs(collection(db, "Peliculas"));
        let docs = [];
        querySnapshot.forEach((doc) => {
            docs.push({ ...doc.data(), id: doc.id });
        });

        films = [...docs];
        console.log(films);
    };
    loadFilm();

    let edit = false;

    const clearFilms = () => {
        film = {
            title: "",
            description: "",
            category: "",
            poster: "",
        };
        edit = false;
    };

    const addFilm = async () => {
        await addDoc(collection(db, "Peliculas"), film);
        await loadFilm();
        clearFilms();
    };

    const saveFilm = async () => {
        await updateDoc(doc(db, "Peliculas", film.id), film);
        await loadFilm();
        clearFilms();
    };

    const editFilm = (f) => {
        film = Object.assign({}, f);
        edit = true;
    };

    const deleteFilm = async (id) => {
        await deleteDoc(doc(db, "Peliculas", id));
        await loadFilm();
    };
    const onSubmitHandlerFilm = (p) => {
        if (edit) {
            console.log("save..");
            saveFilm();
        } else {
            addFilm();
        }
    };
     // FUNCIONES CUSTOMERS

     let customer = {
        name: "",
        lastName: "",
        Category_preference: "",
        img: "",
    };

    let customers = [];

    const loadCustomer = async () => {
        const querySnapshot = await getDocs(collection(db, "Clientes"));
        let docs = [];
        querySnapshot.forEach((doc) => {
            docs.push({ ...doc.data(), id: doc.id });
        });

        customers = [...docs];
        console.log(customers);
    };

    loadCustomer();
    

    

    const clearCustomer = () => {
        customer = {
            name: "",
            lastName: "",
            category_preference: "",
            img: "",
        };
        edit = false;
    };

    const addCustomer = async () => {
        await addDoc(collection(db, "Clientes"), customer);
        await loadCustomer();
        clearCustomer();
    };

    const saveCustomer = async () => {
        await updateDoc(doc(db, "Clientes", customer.id), customer);
        await loadCustomer();
        clearCustomer();
    };

    const editCustomer = (cu) => {
        customer = Object.assign({}, cu);
        edit = true;
    };

    const deleteCustomer = async (id) => {
        await deleteDoc(doc(db, "Clientes", id));
        await loadCustomer();
    };

    const onSubmitHandlerCustomer = (j) => {
        if (edit) {
            console.log("save..");
            saveCustomer();
        } else {
            addCustomer();
        }
    };
</script>
<main class="bg-gray-100">
    <!--VISTA PELICULAS-->
    <h1 class="text-center text-7xl text-color bg-gray-200">FILMS</h1>
    <div class="">
<div class="grid grid-cols-1 gap-2">
<div class="bg-gray-100	grid grid-cols-3 gap-4">
     <!--BUCLE QUE RECORRE LOS OBJETOS PELICULAS-->
{#each films as f, i}
    <div class="bg-gray-300 rounded-lg sahdow-lg overflow-hidden border m-1 flex flex-col md:flex-row">
        <div class="w-full mx-2 h-80 bg-gray-800	 rounded-md p-2 my-2">
            {#if f.poster}
                <img class="object-center object-contain w-full h-full" src={f.poster} alt="thumbnail"/>
            {:else}
                <img class="object-center object-contain w-full h-full" src="images/no-product.jpg" alt="thumbnail"/>
            {/if}
        </div>
        <div class="w-full md:w-3/5 text-left p-6 md:p-4 space-y-2">
            <p class="text-xl text-gray-900 font-bold">
                {f.title}
            </p>
            <p class="text-base text-gray-900 font-normal">
                {f.category}
            </p>
            <p class="text-base leading-relaxed text-gray-800 font-normal">
                {f.description || "Sin descripción"}
            </p>
            <div class="flex justify-start space-x-2">
                <button class="ml-5 bg-gray-200 py-7 px-6 border border-gray-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-gray-300 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-blue-300 focus:text-gray-700 focus:bg-blue-200"
                    on:click={editFilm(f)}>
                    EDIT
                </button>
                <button class="ml-5 bg-gray-800 py-7 px-5 border border-red-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-gray-900 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-red-300 focus:text-gray-700 focus:bg-red-200"
                    on:click={deleteFilm(f.id)}>
                    DELETE
                </button>
                <button class="ml-5 bg-gray-200 py-7 px-5 border border-gray-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-gray-300 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-blue-300 focus:text-gray-700 focus:bg-blue-200" button on:click={rentFilm}>RENT</button>
            </div>
        </div>
    </div>
{/each}
</div>
<!--VISTA  PELICULAS 2-->
<div class="bg-gray-900  p-4 shadow-md rounded-md text-center">
     <!--FORMULARIO PELICULAS-->
<form on:submit|preventDefault={onSubmitHandlerFilm}>
    <label class="text-white" for="nombre">TITLE</label>
    <input
        bind:value={film.title}
        class="text-center"
        id="nombre"
        type="text"
        placeholder="Title of film"
    />
    <label  class="text-white" for="descripcion">DESCRIPTION</label>
    <input
        bind:value={film.description}
        class="text-center"
        id="descripcion"
        type="text"
        placeholder="Description"
    />
    <label class="text-white" for="imagen-url">POSTER</label>
    <input
        bind:value={film.poster}
        class="text-center"
        type="url"
        id="imagen-url"
        placeholder="url"
    />
    <label class="text-white" for="categoria">CATEGORY</label>
    <select bind:value={film.category}   class="text-center" id="categoria">
        <option class="text-color to-black" value=" ACTION">ACTION</option>
        <option value="COMEDY">COMEDY</option>
        <option value="SCIFI">CIENCIE FICTION</option>
        <option value="ROMANTIC">LOVE</option>
        <option value="TERROR">TERROR</option>
        <option value="SUSPENSE">SUSPENSE</option>
    </select>
    <hr class="my-2" />
    {#if edit}
        <button class="ml-5 bg-gray-200 py-8 px-6 border border-gray-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-indigo-500 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-gray-300 focus:text-gray-700 focus:bg-indigo-200 flex items-center">
            EDIT
        </button>
    {:else}
        <button class="ml-5 bg-gray-200 py-8 px-6 border border-gray-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-gray-500 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-green-300 focus:text-gray-700 focus:bg-green-200  flex items-center">
            ADD
        </button>
    {/if}
</form>
</div>
</div>
<!--VISTA CLIENTES-->
<h1 class="text-center text-7xl bg-green-100">CUSTOMERS</h1>
<div class="container mx-auto">
<div class="grid grid-cols-1 gap-2">
<div class=" grid grid-cols-3 gap-4">
     <!--BUCLES PARA RECORRER OBJETOS CLIENTES-->
    {#each customers as cu, o}
        <div class="bg-green-600 rounded-lg sahdow-lg overflow-hidden border m-1 flex flex-col md:flex-row">
            <div class="w-full mx-2 h-80 bg-green-200 rounded-md p-2 my-2">
{#if cu.img}
    <img
        class="object-center object-contain w-full h-full"
        src={cu.img}
        alt="thumbnail"
    />
{:else}
    <img
        class="object-center object-contain w-full h-full"
        src="images/no-product.jpg"
        alt="thumbnail"
    />
{/if}
</div> 
    <div class="w-full md:w-3/5 text-left p-6 md:p-4 space-y-2">
                <p class="text-xl text-green-700 font-bold">
                    {cu.name}
                </p>
                <p class="text-base text-green-400 font-normal">
                    {cu.lastName}
                </p>
                <p  class="text-base leading-relaxed text-green-500 font-normal">
                    {cu.Category_preference}
                </p>
                <div class="flex justify-start space-x-2">
                    <button class="ml-5 bg-gray-200 py-8 px-6 border border-gray-500 rounded-md shadow-sm text-sm leading-4 font-medium text-green-700 hover:bg-gray-500 hover:text-green-100 focus:ring-2 focus:ring-offset-2 focus:ring-blue-300 focus:green-gray-700 focus:bg-blue-200" on:click={editCustomer(cu)}>
                        EDIT
                    </button>
                    <button class="ml-5 bg-red-200 py-8 px-6 border border-red-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-gray-500 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-red-300 focus:text-gray-700 focus:bg-red-200" on:click={deleteCustomer(cu.id)}>
                        DELET
                    </button>
                </div>
            </div>
        </div>
    {/each}
</div>
<!--VISTA CLIENTES 2-->
<div class="bg-gray-800 p-4 shadow-md rounded-md text-center">
     <!--FORMULARIO CLIENTES-->
    <form on:submit|preventDefault={onSubmitHandlerCustomer}>
        <label class="text-white" for="nombre">NAME</label>
        <input
            bind:value={customer.name}
            class="text-center"
            id="nombre"
            type="text"
            placeholder="Name"
        />
        <label class="text-white" for="descripcion">LAST NAME</label>
        <input
            bind:value={customer.lastName}
            class="text-center"
            id="Last name"
            type="text"
            placeholder="Last Name"
        />
        <label class="text-white" for="imagen-url">PHOTO</label>
        <input
            bind:value={customer.img}
            class="text-center"
            type="url"
            id="imagen-url"
            placeholder="url"
        /
        '¡'     3 45789'+çç
        +`
          
        <label class="text-white" for="categoria">PREFERENCE</label>
        <select
            bind:value={customer.Category_preference}
            class="text-center" id="categoria"
        >
            <option value="TERROR">TERROR</option>
            <option value="ACTION">ACTION</option>
            <option value="COMEDY">COMEDY</option>
            <option value="SCIFI">CIENCIIE FICTION</option
            >
            <option value="LOVE">LOVE</option>
        </select>
        <hr class="my-2" />
        {#if edit}
            <button
                class="ml-5 bg-indigo-200 py-8 px-6 border border-indigo-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-indigo-500 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-indigo-300 focus:text-gray-700 focus:bg-indigo-200 flex items-center">
                EDIT
            </button>
        {:else}
            <button
                class="ml-5 bg-green-200 py-8 px-6 border border-green-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-green-500 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-green-300 focus:text-gray-700 focus:bg-green-200  flex items-center">
                ADD
            </button>
        {/if}
    </form>
</div>
 </div>
</div>
</div>
</main>
