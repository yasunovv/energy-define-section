import androidx.compose.desktop.ui.tooling.preview.Preview
import androidx.compose.foundation.Image
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.material.*
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.paint
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.painter.BitmapPainter
import androidx.compose.ui.res.loadImageBitmap
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.res.useResource
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.window.Window
import androidx.compose.ui.window.application
import java.awt.Dimension
import java.awt.Image

@Composable
@Preview
fun App() {
    var voltageInput by remember { mutableStateOf("") }
    var powerInput by remember { mutableStateOf("") }
    var isComputedResult by remember { mutableStateOf(false) }
    var solutionResult by remember { mutableStateOf("") }
    val defaultColor = Color(red = 53, green = 116, blue = 232)
    MaterialTheme () {
        Column (
            modifier = Modifier
                .background(color = Color.White)
        ) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(16.dp)
            ) {
                Text(
                    "Определение оптимального сечения проводника",
                    style = MaterialTheme.typography.h5,
                    fontWeight = FontWeight.Medium
                )
                Spacer(modifier = Modifier.padding(8.dp))
                Text("Расчетная мощность ВЛ 35–500 кВ со сталеалюминиевыми проводами, МВт, при нормированной плотности тока\n" +
                        "\n(при 1,05 номинальном напряжении, Tmax = 5000 ч/год (максимальная нагрузка током), cos φ = 0,9")
                Spacer(modifier = Modifier.padding(8.dp))

                Row {
                    OutlinedTextField(
                        value = voltageInput,
                        onValueChange = {voltageInput = it.filter(Char::isDigit)},
                        label = {
                            Text("Напряжение (кВ)")
                        },
                        placeholder = {
                            Text("220")
                        },
                        colors = TextFieldDefaults.outlinedTextFieldColors(focusedBorderColor = defaultColor, focusedLabelColor = defaultColor)



                    )
                    Spacer(modifier = Modifier.padding(8.dp))

                    OutlinedTextField(
                        value = powerInput,
                        onValueChange = {powerInput = it.filter(Char::isDigit)},
                        label = {
                            Text("Мощность (МВт)")
                        },
                        placeholder = {
                            Text("340")
                        },
                        colors = TextFieldDefaults.outlinedTextFieldColors(focusedBorderColor = defaultColor, focusedLabelColor = defaultColor)
                    )


                }
                Spacer(modifier = Modifier.padding(8.dp))
                Row {
                    Column {
                        Button(onClick = {
                            if (voltageInput != "" && powerInput != ""){
                                solutionResult = findSolution(voltageInput, powerInput)
                                isComputedResult = true
                            }
                        },
                            colors = ButtonDefaults.buttonColors(backgroundColor = defaultColor,
                            )) {
                            Text(
                                "Найти сечение и марку провода",
                                color = Color.White
                            )
                        }
                        Spacer(modifier = Modifier.padding(8.dp))
                        if (isComputedResult){
                            Column {
                                Text("Результат: ", fontWeight = FontWeight.Medium)
                                Text(solutionResult)
                            }

                        }
                    }
                    Spacer(modifier = Modifier.padding(40.dp))
                    Column {
                        Image(
                            painterResource("drawable/image.png"),
                            contentDescription = null,
                            modifier = Modifier.height(300.dp))
                    }
                }

            }

        }

    }
}

// TODO
// иконка
// поставить минимальный размер экрана

fun main() = application {
    Window(
        onCloseRequest = ::exitApplication,

        icon = painterResource("drawable/logo.png"),
        title = "Программа по энергетике"
        ) {
        Box(Modifier.paint(painterResource("drawable/logo.png")).fillMaxSize())
        window.minimumSize = Dimension(800, 600)
        window.maximumSize = Dimension(800, 600)
        App()
    }
}

private fun findSolution(_voltage: String, _power: String): String{
    val voltage = _voltage.toInt()
    val power = _power.toDouble()
    var powerType = 0
    // types
    // 0 - 0..110
    // 1 - 110-150
    // 2 - 150-220
    // 3 - 220-330
    // 4 - 330-infinity
    var powerToSquareArray = mapOf<Double, Int>()
    var message = ""
    // Отбираем значения по вольтажу
    // --> mapOf (power -> square)
    when (voltage) {
        in 0..110 -> {
            powerToSquareArray = mapOf(
                1.7 to 35,
                2.6 to 50,
                3.5 to 70,
                4.8 to 95,
                6.1 to 120,
                7.6 to 150,
                9.3 to 185
            )
            powerType = 0
        }
        in 110..150 -> {
            powerToSquareArray = mapOf(
                11.0 to 70,
                15.0 to 95,
                19.0 to 120,
                23.8 to 150,
                29.2 to 185,
                38.8 to 200,
            )
            powerType = 1
        }
        in 150..220 -> {
            powerToSquareArray = mapOf(
                26.0 to 120,
                32.5 to 150,
                40.0 to 185,
                51.0 to 200,
            )
            powerType = 2
        }
        in 220..330 -> {
            powerToSquareArray = mapOf(
                76.0 to 200,
                95.0 to 300,
                127.0 to 400,
                156.0 to 500,
            )
            powerType = 3
        }
        in 330..500 -> {
            powerToSquareArray = mapOf(
                230.0 to 200,
                285.0 to 300,
                379.0 to 400,
                475.0 to 500,
            )
            powerType = 4
        }
        else -> {
            powerToSquareArray = mapOf(
                650.0 to 300,
                715.0 to 330,
                870.0 to 400,
                1080.0 to 500,
            )
            powerType = 5
        }
    }
    message = when (powerType) {
        0 -> {
            when (power){
                in 0.0..1.7 -> {
                    "35 мм^2"
                }
                in 1.7..2.6 -> {
                    "50 мм^2"
                }
                in 2.6..3.5 -> {
                    "70 мм^2"
                }
                in 3.5..4.8 ->{
                    "95 мм^2"
                }
                in 4.8..6.1 ->{
                    "95 мм^2"
                }
                in 6.1..7.6 ->{
                    "95 мм^2"
                }
                in 7.6..9.3 ->{
                    "95 мм^2"
                }
                else -> {
                    "Введите мощность до 9,3 МВТ"
                }
            }
        }
        1 -> {
            when(power){
                in 0.0..11.0 -> {
                    "70 мм^2"
                }
                in 11.0..15.0 -> {
                    "95 мм^2"
                }
                in 15.0..19.0 -> {
                    "120 мм^2"
                }
                in 19.0..23.8 -> {
                    "150 мм^2"
                }
                in 23.8..29.2 -> {
                    "185 мм^2"
                }
                in 29.2..38.8 -> {
                    "200 мм^2 при 1 фазе"
                }

                else -> {
                    "Введите мощность до 38.8 MBт"
                }
            }
        }
        2 -> {
            when(power){
                in 0.0..26.0 -> {
                    "120 мм^2"
                }
                in 26.0..32.5 -> {
                    "150 мм^2"
                }
                in 32.5..40.0 -> {
                    "185 мм^2"
                }
                in 40.0..51.0 -> {
                    "200 мм^2 при 1 фазах"
                }
                else -> {
                    "Введите мощность до 51 MBт"
                }
            }
        }
        3 -> {
            when(power){
                in 0.0..76.0 -> {
                    "200 мм^2 при 1 фазе"
                }
                in 76.0..95.0 -> {
                    "300 мм^2 при 1 фазе"
                }
                in 95.0..127.0 -> {
                    "400 мм^2 при 1 фазе"
                }
                in 127.0..156.0 -> {
                    "500 мм^2 при 1 фазе"
                }
                else -> {
                    "Введите мощность до 156 MBт"
                }
            }

        }
        4 -> {
            when(power){
                in 0.0..230.0 -> {
                    "200 мм^2 при 2 фазах"
                }
                in 230.0..285.0 -> {
                    "300 мм^2 при 2 фазе"
                }
                in 285.0..379.0 -> {
                    "400 мм^2 при 2 фазах"
                }
                in 379.0..475.0 -> {
                    "500 мм^2 при 2 фазах"
                }
                else -> {
                    "Введите мощность до 475 MBт"
                }
            }
        }
        5 -> {
            when(power){
                in 0.0..650.0 -> {
                    "300 мм^2 при 3 фазах"
                }
                in 650.0..715.0 -> {
                    "330 мм^2 при 3 фазах"
                }
                in 715.0..870.0 -> {
                    "400 мм^2 при 3 фазах"
                }
                in 870.0..1080.0 -> {
                    "500 мм^2 при 3 фазах"
                }
                else -> {
                    "Введите мощность до 1080 MBт"
                }
            }
        }
        else -> {"ОШИБКА"}
    }
    return message
}