from PIL import Image
from pathlib import Path

def process_image():
    filename = input("Enter image path: ")
    path = Path(filename)

    if not path.exists():
        print("File not found!")
        return

    try:
        image = Image.open(path)

        print("\n1. Convert format")
        print("2. Resize image")
        print("3. Optimize image")

        choice = input("Choose: ")

        if choice == "1":
            fmt = input("Target format (PNG/JPEG/WEBP): ").upper()

            if fmt not in ["PNG", "JPEG", "WEBP"]:
                print("Unsupported format.")
                return

            output = path.with_suffix("." + fmt.lower())

            converted = image.convert("RGB") if fmt == "JPEG" else image
            converted.save(output, format=fmt)

            print(f"Saved: {output}")

        elif choice == "2":
            width = int(input("New width: "))
            height = int(input("New height: "))

            if width <= 0 or height <= 0:
                print("Dimensions must be positive.")
                return

            resized = image.resize((width, height))
            output = path.with_name(path.stem + "_resized.png")
            resized.save(output)

            print(f"Saved: {output}")

        elif choice == "3":
            output = path.with_name(path.stem + "_optimized.jpg")
            image.convert("RGB").save(
                output,
                "JPEG",
                quality=75,
                optimize=True
            )

            print(f"Saved: {output}")

        else:
            print("Invalid choice.")

    except Exception as error:
        print("Error:", error)

if __name__ == "__main__":
    process_image()
