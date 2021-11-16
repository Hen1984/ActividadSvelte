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
