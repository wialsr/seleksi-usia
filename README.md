
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Seleksi Usia Pengunjung</title>
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            min-height: 100vh;
            background: #f5f5f5;
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            width: 650px;
            max-width: 95%;
            background: white;
            border: 1px solid #555;
            padding: 35px;
            border-radius: 5px;
            box-shadow: 0 3px 12px #00000015;
        }

        h1 {
            color: #35006d;
            text-decoration: underline;
            text-align: center;
            margin-top: 0;
            margin-bottom: 35px;
            font-size: 32px;
        }

        .form-group {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
        }

        .form-group label {
            width: 100px;
            font-size: 23px;
        }

        .form-group input {
            flex: 1;
            min-width: 0;
            height: 43px;
            padding: 8px 12px;
            font-size: 20px;
            border: 2px solid #555;
            border-radius: 5px;
            outline: none;
        }

        .form-group input:focus {
            border-color: #35006d;
        }

        .buttons {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 30px;
        }

        button {
            padding: 10px 20px;
            font-size: 19px;
            font-weight: bold;
            cursor: pointer;
            border: 2px solid #555;
            border-radius: 5px;
            background: #d8d8d8;
            color: #222;
            box-shadow: 2px 3px 0 #888;
        }

        button:hover {
            background: #c3b2d8;
        }

        .note {
            text-align: center;
            margin-top: 25px;
            color: #ff1680;
            font-size: 15px;
            font-weight: bold;
        }

        /* Dialog hasil */
        .overlay {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.35);
            justify-content: center;
            align-items: center;
            padding: 15px;
        }

        .dialog {
            background: white;
            width: 390px;
            max-width: 100%;
            border: 2px solid #444;
            border-radius: 5px;
            box-shadow: 0 4px 15px #00000035;
            animation: muncul 0.2s ease;
        }

        .dialog-header {
            display: flex;
            justify-content: flex-end;
            padding: 8px 10px;
            border-bottom: 1px solid #ddd;
        }

        .close {
            border: none;
            box-shadow: none;
            background: transparent;
            padding: 2px 8px;
            font-size: 23px;
        }

        .dialog-body {
            padding: 15px 22px;
            font-size: 22px;
            line-height: 1.5;
            overflow-wrap: anywhere;
        }

        .dialog-footer {
            text-align: center;
            padding: 0 15px 20px;
        }

        .ok-button {
            min-width: 75px;
        }

        @keyframes muncul {
            from {
                opacity: 0;
                transform: scale(0.95);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }

        @media (max-width: 480px) {
            .container {
                padding: 22px 16px;
            }

            h1 {
                font-size: 27px;
            }

            .form-group label {
                width: 75px;
                font-size: 19px;
            }

            .form-group input {
                font-size: 17px;
            }

            .buttons {
                gap: 12px;
            }

            button {
                padding: 10px 14px;
                font-size: 16px;
            }
        }
    </style>
</head>

<body>
    <div class="container">

        <form id="formUsia">
            <div class="form-group">
                <label for="nama">Nama</label>
                <input
                    type="text"
                    id="nama"
                    placeholder="Masukkan nama"
                    autocomplete="off"
                >
            </div>

            <div class="form-group">
                <label for="usia">Usia</label>
                <input
                    type="number"
                    id="usia"
                    placeholder="Masukkan usia"
                    min="0"
                    step="1"
                >
            </div>

            <div class="buttons">
                <button type="submit">PROSES</button>
                <button type="button" id="clear">CLEAR</button>
            </div>
        </form>

    <!-- Dialog alert buatan -->
    <div class="overlay" id="overlay">
        <div class="dialog" role="alertdialog"
             aria-modal="true" aria-labelledby="pesan">
            <div class="dialog-header">
                <button type="button" class="close"
                        id="tutup" aria-label="Tutup">×</button>
            </div>

            <div class="dialog-body" id="pesan"></div>

            <div class="dialog-footer">
                <button type="button" class="ok-button" id="ok">
                    OK
                </button>
            </div>
        </div>
    </div>

    <!-- jQuery -->
    <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>

    <script>
        $(document).ready(function () {

            // Fungsi untuk menampilkan dialog
            function tampilkanDialog(pesan) {
                $("#pesan").text(pesan);
                $("#overlay").css("display", "flex");
                $("#ok").trigger("focus");
            }

            // Fungsi untuk menutup dialog
            function tutupDialog() {
                $("#overlay").hide();
            }

            // Tombol PROSES
            $("#formUsia").on("submit", function (event) {
                event.preventDefault();

                let nama = $("#nama").val().trim();
                let usiaText = $("#usia").val().trim();

                if (nama === "" || usiaText === "") {
                    tampilkanDialog("Nama dan usia harus diisi!");
                    return;
                }

                let usia = Number(usiaText);

                if (!Number.isInteger(usia) || usia < 0) {
                    tampilkanDialog("Usia harus berupa bilangan bulat yang valid!");
                    return;
                }

                if (usia >= 27) {
                    tampilkanDialog(
                        "HELLO, " + nama.toUpperCase() +
                        "\nANDA BOLEH MASUK"
                    );
                } else {
                    tampilkanDialog(
                        "HELLO, " + nama.toUpperCase() +
                        "\nANDA TIDAK BOLEH MASUK"
                    );
                }
            });

            // Tombol CLEAR
            $("#clear").on("click", function () {
                $("#nama").val("");
                $("#usia").val("");

                tampilkanDialog("Nama & usia terhapus.");
                $("#nama").trigger("focus");
            });

            // Tombol OK dan tanda X
            $("#ok, #tutup").on("click", function () {
                tutupDialog();
            });

            // Menutup dialog dengan tombol Escape
            $(document).on("keydown", function (event) {
                if (event.key === "Escape") {
                    tutupDialog();
                }
            });

        });
    </script>

</body>
</html>
