
# Pack démo – Mariage de Yvana (6 tables)

## Contenu
- index.html (accueil + saisie du nom)
- table1.html … table6.html (pages de table)
- style.css (thème vert & rose)
- README_QR.txt (ce fichier)

## Hébergement
Déposez ces fichiers tels quels sur un hébergement statique (GitHub Pages, Netlify, Vercel, ou un simple serveur web). 
L’URL publique de `index.html` sera la cible du QR.

Exemple d’URL cible (à adapter) :
    https://exemple.com/mariage/index.html

## Génération du QR (Java ZXing)
Ajoutez ZXing à votre build (Maven ou Gradle), puis utilisez le code suivant en remplaçant URL_CIBLE par l’URL de votre index.html :

--- Java ---
import com.google.zxing.BarcodeFormat;
import com.google.zxing.EncodeHintType;
import com.google.zxing.client.j2se.MatrixToImageWriter;
import com.google.zxing.common.BitMatrix;
import com.google.zxing.qrcode.QRCodeWriter;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;
import java.nio.file.Paths;
import java.util.EnumMap;
import java.util.Map;

public class QrGenerator {
  public static void main(String[] args) throws Exception {
    String url = "URL_CIBLE"; // ex: https://exemple.com/mariage/index.html
    int size = 1024; // grande taille pour impression nette
    QRCodeWriter writer = new QRCodeWriter();
    Map<EncodeHintType,Object> hints = new EnumMap<>(EncodeHintType.class);
    hints.put(EncodeHintType.CHARACTER_SET, "UTF-8");
    hints.put(EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.M); // H si vous ajoutez un logo
    hints.put(EncodeHintType.MARGIN, 2);
    BitMatrix matrix = writer.encode(url, BarcodeFormat.QR_CODE, size, size, hints);
    MatrixToImageWriter.writeToPath(matrix, "PNG", Paths.get("qr_code.png"));
    System.out.println("QR généré → qr_code.png");
  }
}
---

Pour un **SVG**, utilisez le module `javase` de ZXing et un utilitaire comme `MatrixToSvgWriter` (facile à ajouter) ou exportez en PNG >300 DPI.

## Impression
- Imprimez en 4–6 cm de côté (PNG 1024 px = parfait à 300 DPI).
- Laissez une marge blanche autour.
- Testez avec plusieurs téléphones.
