# rail-fence-cipher-index.html
<!DOCTYPE html>
<html>
<head>

<title>Rail Fence Cipher</title>

<style>

body {
    font-family: Arial;
    background: #f2f2f2;
    text-align: center;
    padding: 40px;
}

.container {
    background: white;
    width: 500px;
    margin: auto;
    padding: 30px;
    border-radius: 10px;
}

textarea {
    width: 100%;
    height: 120px;
    margin: 10px 0;
}

input {
    padding: 10px;
    width: 100%;
    box-sizing: border-box;
}

button {
    padding: 10px 20px;
    margin: 10px;
}

</style>

</head>

<body>

<div class="container">

<h1>Rail Fence Cipher</h1>

<textarea id="input" placeholder="Masukkan teks"></textarea>

<input 
    type="number" 
    id="rails" 
    value="3"
    min="2"
    placeholder="Jumlah Rail"
>

<br>

<button onclick="encrypt()">Enkripsi</button>

<button onclick="decrypt()">Dekripsi</button>

<textarea id="result" placeholder="Hasil" readonly></textarea>

</div>

<script>

function encrypt() {

    let text = document.getElementById("input").value;
    let rails = parseInt(document.getElementById("rails").value);

    if (rails < 2) {
        alert("Rail minimal 2");
        return;
    }

    let fence = Array.from(
        {length: rails},
        () => []
    );

    let row = 0;
    let direction = 1;

    for (let char of text) {

        fence[row].push(char);

        if (row === 0)
            direction = 1;

        if (row === rails - 1)
            direction = -1;

        row += direction;
    }

    document.getElementById("result").value =
        fence.flat().join("");
}


function decrypt() {

    let text = document.getElementById("input").value;
    let rails = parseInt(document.getElementById("rails").value);

    let pattern = [];

    let row = 0;
    let direction = 1;

    for (let i = 0; i < text.length; i++) {

        pattern.push(row);

        if (row === 0)
            direction = 1;

        if (row === rails - 1)
            direction = -1;

        row += direction;
    }

    let counts = Array(rails).fill(0);

    pattern.forEach(r => counts[r]++);

    let rail = [];
    let index = 0;

    for (let count of counts) {

        rail.push(
            text.slice(index, index + count).split("")
        );

        index += count;
    }

    let position = Array(rails).fill(0);
    let result = "";

    for (let r of pattern) {

        result += rail[r][position[r]];
        position[r]++;
    }

    document.getElementById("result").value = result;
}

</script>

</body>
</html>
