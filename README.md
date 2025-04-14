- 👋// Proyecto Android básico con Jetpack Compose y Firebase (tema oscuro)

// build.gradle (nivel de módulo) dependencies { implementation 'androidx.core:core-ktx:1.12.0' implementation 'androidx.lifecycle:lifecycle-runtime-ktx:2.6.2' implementation 'androidx.activity:activity-compose:1.8.2' implementation 'androidx.compose.ui:ui:1.5.4' implementation 'androidx.compose.material3:material3:1.1.2' implementation 'androidx.compose.ui:ui-tooling-preview:1.5.4' implementation 'com.google.firebase:firebase-bom:32.3.1' implementation 'com.google.firebase:firebase-firestore-ktx' implementation 'com.google.firebase:firebase-analytics-ktx' implementation 'androidx.navigation:navigation-compose:2.7.6' }

// AndroidManifest.xml <uses-permission android:name="android.permission.INTERNET" />

// MainActivity.kt @Composable fun TaxiApp() { MaterialTheme(colorScheme = darkColorScheme()) { Surface(modifier = Modifier.fillMaxSize()) { TaxiFormScreen() } } }

// TaxiFormScreen.kt @Composable fun TaxiFormScreen() { var nombre by remember { mutableStateOf("") } var taxiSeleccionado by remember { mutableStateOf("") } val taxis = listOf("Taxi 704", "Taxi 437", "Taxi 610", "Taxi 392") var liquido by remember { mutableStateOf(false) } var monto by remember { mutableStateOf("") } var formaPago by remember { mutableStateOf("") } var kilometraje by remember { mutableStateOf("") } var observaciones by remember { mutableStateOf("") } val estadoVehiculo = remember { mutableStateMapOf( "Aceite" to false, "Llantas" to false, "Interior" to false, "Carrocería" to false ) }

Column(modifier = Modifier.padding(16.dp)) {
    Text("Registro Diario", style = MaterialTheme.typography.headlineSmall)

    OutlinedTextField(value = nombre, onValueChange = { nombre = it }, label = { Text("Nombre del conductor") })

    DropdownMenuTaxi(taxis, taxiSeleccionado) { taxiSeleccionado = it }

    Row(verticalAlignment = Alignment.CenterVertically) {
        Checkbox(checked = liquido, onCheckedChange = { liquido = it })
        Text("¿Liquidó hoy?")
    }

    OutlinedTextField(value = monto, onValueChange = { monto = it }, label = { Text("Monto liquidado") })
    OutlinedTextField(value = formaPago, onValueChange = { formaPago = it }, label = { Text("Forma de pago") })

    Text("Estado del vehículo")
    estadoVehiculo.forEach { (label, checked) ->
        Row(verticalAlignment = Alignment.CenterVertically) {
            Checkbox(checked = checked, onCheckedChange = { estadoVehiculo[label] = it })
            Text(label)
        }
    }

    OutlinedTextField(value = kilometraje, onValueChange = { kilometraje = it }, label = { Text("Kilometraje") })
    OutlinedTextField(value = observaciones, onValueChange = { observaciones = it }, label = { Text("Observaciones") })

    Button(onClick = {
        guardarEnFirebase(
            nombre, taxiSeleccionado, liquido, monto, formaPago,
            estadoVehiculo.filterValues { it }.keys.toList(), kilometraje, observaciones
        )
    }) {
        Text("Enviar")
    }
}

}

@Composable fun DropdownMenuTaxi(options: List<String>, selected: String, onSelect: (String) -> Unit) { var expanded by remember { mutableStateOf(false) }

Box {
    OutlinedTextField(
        value = selected,
        onValueChange = {},
        label = { Text("Seleccione taxi") },
        readOnly = true,
        modifier = Modifier.clickable { expanded = true }
    )
    DropdownMenu(expanded = expanded, onDismissRequest = { expanded = false }) {
        options.forEach {
            DropdownMenuItem(onClick = {
                onSelect(it)
                expanded = false
            }, text = { Text(it) })
        }
    }
}

}

fun guardarEnFirebase( nombre: String, taxi: String, liquido: Boolean, monto: String, formaPago: String, estadoVehiculo: List<String>, kilometraje: String, observaciones: String ) { val db = Firebase.firestore val registro = hashMapOf( "nombre" to nombre, "taxi" to taxi, "liquido" to liquido, "monto" to monto, "formaPago" to formaPago, "estadoVehiculo" to estadoVehiculo, "kilometraje" to kilometraje, "observaciones" to observaciones, "fecha" to System.currentTimeMillis() ) db.collection("registros").add(registro) }

 Hi, I’m @Negro124092
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Negro124092/Negro124092 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
