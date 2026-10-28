
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
            width: 800px;
            max-width: 95%;
            padding: 35px;
            background: white;
            border: 1px solid #777;
            border-radius: 6px;
            box-shadow: 0 3px 12px #00000015;
        }

        h1 {
            margin: 0 0 35px;
            color: #35006d;
            font-size: 32px;
            text-align: center;
            text-decoration: underline;
        }

        .form-group {
            display: flex;
            align-items: center;
            margin-bottom: 25px;
        }

        .form-group label {
            width: 140px;
            flex-shrink: 0;
            font-size: 23px;
        }

        .form-group input {
            flex: 1;
            min-width: 0;
            height: 55px;
            padding: 10px 15px;
            font-size: 20px;
            border: 2px solid #555;
            border-radius: 6px;
        }

        .form-group input:focus {
            outline: 2px solid #35006d;
            outline-offset: 2px;
        }

        .buttons {
            display: flex;
            justify-content: center;
            gap: 25px;
            margin-top: 35px;
        }

        button {
            padding: 12px 25px;
            font-size: 19px;
            font-weight: bold;
            cursor: pointer;
            color: #222;
            background: #d8d8d8;
            border: 2px solid #555;
            border-radius: 6px;
            box-shadow: 2px 3px 0 #888;
        }

        button:hover {
            background: #c3b2d8;
        }

        .note {
            margin-top: 28px;
            color: #ff1680;
            font-size: 15px;
            font-weight: bold;
            text-align: center;
        }

        /* Dialog */
        .overlay {
            display: none;
            position: fixed;
            inset: 0;
            z-index: 9999;
            justify-content: center;
            align-items: center;
            padding: 15px;
            background: rgba(0, 0, 0, 0.35);
        }

        .dialog {
            width: 390px;
            max-width: 100%;
            background: white;
            border: 2px solid #444;
            border-radius: 6px;
            box-shadow: 0 4px 15px #00000035;
        }

        .dialog-header {
            display: flex;
            justify-content: flex-end;
            padding: 5px 10px;
            border-bottom: 1px solid #ddd;
        }

        .close {
            padding: 2px 8px;
            font-size: 24px;
            background: white;
            border: none;
            box-shadow: none;
        }

        .dialog-body {
            padding: 20px;
            font-size: 22px;
            line-height: 1.5;
            overflow-wrap: anywhere;
        }

        .dialog-footer {
            padding: 0 15px 20px;
            text-align: center;
        }

        .ok-button {
            min-width: 75px;
        }

        @media (max-width: 480px) {
            .container {
                padding: 22px 16px;
            }

            h1 {
                font-size: 25px;
            }

            .form-group label {
                width: 75px;
                font-size: 18px;
            }

            .form-group input {
                font-size: 16px;
            }

            .buttons {
                gap: 15px;
            }

            button {
                padding: 10px 15px;
                font-size: 16px;
            }
        }
    </style>
</head>

<body>

    <div class="container">
        <h1>Seleksi Usia Pengunjung</h1>

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

        <div class="note">
            Usia minimal 27 tahun untuk boleh masuk.
        </div>
    </div>

    <!-- Dialog hasil -->
    <div class="overlay" id="overlay">
        <div class="dialog" role="alertdialog"
             aria-modal="true" aria-labelledby="pesan">

            <div class="dialog-header">
                <button type="button" class="close" id="tutup"
                        aria-label="Tutup">×</button>
            </div>

            <div class="dialog-body" id="pesan"></div>

            <div class="dialog-footer">
                <button type="button" class="ok-button" id="ok">
                    OK
                </button>
            </div>

        </div>
    </div>

    <!-- Library jQuery -->
    <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>

    <script>
        $(document).ready(function () {

            // Menampilkan dialog
            function tampilkanDialog(pesan) {
                $("#pesan").text(pesan);
                $("#overlay").css("display", "flex");
            }

            // Menutup dialog
            function tutupDialog() {
                $("#overlay").hide();
            }

            // Tombol PROSES
            $("#formUsia").on("submit", function (event) {
                event.preventDefault();

                const nama = $("#nama").val().trim();
                const usiaText = $("#usia").val().trim();

                if (nama === "" || usiaText === "") {
                    tampilkanDialog("Nama dan usia harus diisi!");
                    return;
                }

                const usia = Number(usiaText);

                if (!Number.isInteger(usia) || usia < 0) {
                    tampilkanDialog(
                        "Usia harus berupa bilangan bulat yang valid!"
                    );
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

                tampilkanDialog("Nama dan usia terhapus.");
                $("#nama").trigger("focus");
            });

            // Tombol OK dan X
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
