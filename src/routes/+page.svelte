<script lang="ts">
    import { jsPDF } from "jspdf";
    import autoTable from "jspdf-autotable";

    const fechaHoy = new Date();
    const año = fechaHoy.getFullYear();
    const mes = fechaHoy.getMonth() + 1;
    const dia = fechaHoy.getDate();
    const fechaFormateada = `${año}-${mes < 10 ? "0" + mes : mes}-${
        dia < 10 ? "0" + dia : dia
    }`;

    let usuarios = [
        {
            id: 1,
            name: "Juan Pérez",
            email: "Juan@Example.com",
        },
    ];
    let nuevoUsuario = {
        id: 2,
        name: "",
        email: "",
    };
    function agregar() {
        const usuarioNuevo = { ...nuevoUsuario };
        usuarios = [...usuarios, usuarioNuevo];
        nuevoUsuario.id++;
        console.log(usuarios);
        nuevoUsuario.name = "";
        nuevoUsuario.email = "";
    }

    function urlPDF() {
        const doc = new jsPDF();
        doc.text("Pagina a generar:", 10, 10);
        doc.text(fechaFormateada, 170, 10);
        doc.setLineWidth(0.75);
        doc.line(10, 13, 200, 13);
        autoTable(doc, {
            margin: 20,
            head: [["Id", "Nombre", "Email"]],
            body: usuarios.map((x) => {
                return [x.id.toString(), x.name, x.email];
            }),
        });
        doc.text(
            "© 2024 Informe Simple. Todos los derechos reservados.",
            10,
            290,
        );
        doc.autoPrint({ variant: "non-conform" });
        doc.save("autoprint.pdf");
    }
    function generarPDF() {
        const doc = new jsPDF();
        doc.text("Pagina a generar:", 10, 10);
        doc.text(fechaFormateada, 170, 10);
        doc.setLineWidth(0.75);
        doc.line(10, 13, 200, 13);
        autoTable(doc, {
            margin: 20,
            head: [["Id", "Nombre", "Email"]],
            body: usuarios.map((x) => {
                return [x.id.toString(), x.name, x.email];
            }),
        });
        doc.text(
            "© 2024 Informe Simple. Todos los derechos reservados.",
            10,
            290,
        );
        doc.save("ListaUsuarios.pdf");
        return;
    }
</script>

<div class="container mx-auto py-4">
    <h1 class="text-center text-3xl">PDF-Printer</h1>
    <div class="mx-auto w-1/4 my-10 p-5 bg-slate-200 rounded shadow-lg">
        <h1 class="text-2xl pb-2">Nuevos usuarios</h1>
        <div class="w-full">
            <p>Nombre:</p>
            <input
                type="text"
                class="input border w-full px-1"
                bind:value={nuevoUsuario.name}
            />
        </div>

        <div class="w-full">
            <p>Email:</p>
            <input
                type="text"
                class="input border w-full px-1"
                bind:value={nuevoUsuario.email}
            />
        </div>
        <button
            on:click={agregar}
            class="bg-blue-500 text-white px-4 py-2 rounded-md w-full mt-3 hover:bg-blue-700 focus:outline-none focus:shadow-outline-blue active:bg-blue-800"
            >Agregar</button
        >
    </div>
    <div class="flex gap-2 justify-center py-5">
        <button
            on:click={urlPDF}
            class="bg-blue-500 text-white px-4 py-2 rounded-full hover:bg-blue-700 focus:outline-none focus:shadow-outline-blue active:bg-blue-800 w-32"
            >Download and print</button
        >
        <button
            on:click={generarPDF}
            class="bg-red-500 text-white px-4 py-2 rounded-full hover:bg-red-700 focus:outline-none focus:shadow-outline-blue active:bg-red-800 w-32"
            >Download</button
        >
    </div>
</div>
<div class="w-8/12 mx-auto rounded-md border-slate-800 border p-5">
    <div class="header flex justify-between">
        <h1 class="text-xl">Pagina a generar:</h1>
        <p>Fecha: <strong>17 de enero de 2024</strong></p>
    </div>
    <br />
    <hr />

    <div class="container mx-auto py-4">
        <table class="min-w-full bg-white border border-gray-300">
            <thead>
                <tr>
                    <th class="py-2 px-4 border-b text-start">ID</th>
                    <th class="py-2 px-4 border-b text-start">Nombre</th>
                    <th class="py-2 px-4 border-b text-start"
                        >Correo Electrónico</th
                    >
                </tr>
            </thead>
            <tbody>
                {#each usuarios as usuario}
                    <tr>
                        <td class="py-2 px-4 border-b">{usuario.id}</td>
                        <td class="py-2 px-4 border-b">{usuario.name}</td>
                        <td class="py-2 px-4 border-b">{usuario.email}</td>
                    </tr>
                {/each}
            </tbody>
        </table>
        <footer class="py-3">
            <p>&copy; 2024 Informe Simple. Todos los derechos reservados.</p>
        </footer>
    </div>
</div>
