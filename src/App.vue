<script setup>
import { ref } from "vue";
import BotonCopiar from "./assets/components/BotonCopiar.vue";
import BotonSwitchTheme from "./assets/components/BotonSwitchTheme.vue";

const inputUser = ref("");
const isFocused = ref(false);

function onFocus() {
  isFocused.value = true;
}

function onBlur() {
  isFocused.value = false;
}

function switchTheme() {
  document.body.classList.toggle("dark-theme");
}

async function copiarTexto(texto) {
  try {
    await navigator.clipboard.writeText(texto);
  } catch (error) {
    alert("No tengo permiso para copiar el texto");
  }
}

function toFont(text, toFontName) {
  const normalFont = FONTS.find((f) => f.name.toLowerCase() === "normal");
  const targetFont = FONTS.find(
    (f) => f.name.toLowerCase() === toFontName.toLowerCase()
  );

  if (!normalFont || !targetFont) return text;

  const normalChars = Array.from(normalFont.chars);
  const targetChars = Array.from(targetFont.chars);

  const charMap = {};
  for (let i = 0; i < normalChars.length; i++) {
    charMap[normalChars[i]] = targetChars[i] || normalChars[i];
  }

  // Descompone texto: "é" → "e" + "◌́"
  const decomposed = text.normalize("NFD");

  let result = "";
  for (const char of decomposed) {
    // Si es un acento (combining mark), lo dejamos.
    if (/[\u0300-\u036f]/.test(char)) {
      result += char;
    } else {
      result += charMap[char] || char;
    }
  }

  // Volver a recomponer los caracteres acentuados.
  return result.normalize("NFC");
}

const FONTS = [
  {
    name: "Normal",
    chars:
      "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!?(){}[]@#$%&*+-=<>áéíóúÁÉÍÓÚñÑçÇ",
  },
  {
    name: "Bold",
    chars:
      "𝐀𝐁𝐂𝐃𝐄𝐅𝐆𝐇𝐈𝐉𝐊𝐋𝐌𝐍𝐎𝐏𝐐𝐑𝐒𝐓𝐔𝐕𝐖𝐗𝐘𝐙𝐚𝐛𝐜𝐝𝐞𝐟𝐠𝐡𝐢𝐣𝐤𝐥𝐦𝐧𝐨𝐩𝐪𝐫𝐬𝐭𝐮𝐯𝐰𝐱𝐲𝐳𝟎𝟏𝟐𝟑𝟒𝟓𝟔𝟕𝟖𝟗!?(){}[]@#$%&*+-=<>𝐚́𝐞́𝐢́𝐨́𝐮́𝐀́𝐄́𝐈́𝐎́𝐔́𝐧̃𝐍̃𝐜̧𝐂̧",
  },
  {
    name: "Italic",
    chars:
      "𝘈𝘉𝘊𝘋𝘌𝘍𝘎𝘏𝘐𝘑𝘒𝘓𝘔𝘕𝘖𝘗𝘘𝘙𝘚𝘛𝘜𝘝𝘞𝘟𝘠𝘡𝘢𝘣𝘤𝘥𝘦𝘧𝘨𝘩𝘪𝘫𝘬𝘭𝘮𝘯𝘰𝘱𝘲𝘳𝘴𝘵𝘶𝘷𝘸𝘹𝘺𝘻0123456789!?(){}[]@#$%&*+-=<>𝘢́𝘦́𝘪́𝘰́𝘶́𝘈́𝘌́𝘐́𝘖́𝘜́𝘯̃𝘕̃𝘤̧𝘊̧",
  },
  {
    name: "Bold Italic",
    chars:
      "𝘼𝘽𝘾𝘿𝙀𝙁𝙂𝙃𝙄𝙅𝙆𝙇𝙈𝙉𝙊𝙋𝙌𝙍𝙎𝙏𝙐𝙑𝙒𝙓𝙔𝙕𝙖𝙗𝙘𝙙𝙚𝙛𝙜𝙝𝙞𝙟𝙠𝙡𝙢𝙣𝙤𝙥𝙦𝙧𝙨𝙩𝙪𝙫𝙬𝙭𝙮𝙯0123456789!?(){}[]@#$%&*+-=<>𝙖́𝙚́𝙞́𝙤́𝙪́𝘼́𝙀́𝙄́𝙊́𝙐́𝙣̃𝙉̃𝙘̧𝘾̧",
  },
  {
    name: "Monospace",
    chars:
      "𝙰𝙱𝙲𝙳𝙴𝙵𝙶𝙷𝙸𝙹𝙺𝙻𝙼𝙽𝙾𝙿𝚀𝚁𝚂𝚃𝚄𝚅𝚆𝚇𝚈𝚉𝚊𝚋𝚌𝚍𝚎𝚏𝚐𝚑𝚒𝚓𝚔𝚕𝚖𝚗𝚘𝚙𝚚𝚛𝚜𝚝𝚞𝚟𝚠𝚡𝚢𝚣𝟶𝟷𝟸𝟹𝟺𝟻𝟼𝟽𝟾𝟿!?(){}[]@#$%&*+-=<>𝚊́𝚎́𝚒́𝚘́𝚞́𝙰́𝙴́𝙸́𝙾́𝚄́𝚗̃𝙽̃𝚌̧𝙲̧",
  },
  {
    name: "Double Struck",
    chars:
      "𝔸𝔹ℂ𝔻𝔼𝔽𝔾ℍ𝕀𝕁𝕂𝕃𝕄ℕ𝕆ℙℚℝ𝕊𝕋𝕌𝕍𝕎𝕏𝕐ℤ𝕒𝕓𝕔𝕕𝕖𝕗𝕘𝕙𝕚𝕛𝕜𝕝𝕞𝕟𝕠𝕡𝕢𝕣𝕤𝕥𝕦𝕧𝕨𝕩𝕪𝕫𝟘𝟙𝟚𝟛𝟜𝟝𝟞𝟟𝟠𝟡!?(){}[]@#$%&*+-=<>𝕒́𝕖́𝕚́𝕠́𝕦́𝔸́𝔼́𝕀́𝕆́𝕌́𝕟̃ℕ̃𝕔̧ℂ̧",
  },
  {
    name: "Bubble",
    chars:
      "ⒶⒷⒸⒹⒺⒻⒼⒽⒾⒿⓀⓁⓂⓃⓄⓅⓆⓇⓈⓉⓊⓋⓌⓍⓎⓏⓐⓑⓒⓓⓔⓕⓖⓗⓘⓙⓚⓛⓜⓝⓞⓟⓠⓡⓢⓣⓤⓥⓦⓧⓨⓩ⓪①②③④⑤⑥⑦⑧⑨!?(){}[]@#$%&*+-=<>ⓐ́ⓔ́ⓘ́ⓞ́ⓤ́Ⓐ́Ⓔ́Ⓘ́Ⓞ́Ⓤ́ⓝ̃Ⓝ̃ⓒ̧Ⓒ̧",
  },
  {
    name: "Fraktur",
    chars:
      "𝔄𝔅ℭ𝔇𝔈𝔉𝔊ℌℑ𝔍𝔎𝔏𝔐𝔑𝔒𝔓𝔔ℜ𝔖𝔗𝔘𝔙𝔚𝔛𝔜ℨ𝔞𝔟𝔠𝔡𝔢𝔣𝔤𝔥𝔦𝔧𝔨𝔩𝔪𝔫𝔬𝔭𝔮𝔯𝔰𝔱𝔲𝔳𝔴𝔵𝔶𝔷օյշՅկՏճԴՑգ!?(){}[]@#$%&*+-=<>𝔞́𝔢́𝔦́𝔬́𝔲́𝔄́𝔈́ℑ́𝔒́𝔘́𝔫̃𝔑̃𝔠̧ℭ̧",
  },
  {
    name: "Bold Fraktur",
    chars:
      "𝕬𝕭𝕮𝕯𝕰𝕱𝕲𝕳𝕴𝕵𝕶𝕷𝕸𝕹𝕺𝕻𝕼𝕽𝕾𝕿𝖀𝖁𝖂𝖃𝖄𝖅𝖆𝖇𝖈𝖉𝖊𝖋𝖌𝖍𝖎𝖏𝖐𝖑𝖒𝖓𝖔𝖕𝖖𝖗𝖘𝖙𝖚𝖛𝖜𝖝𝖞𝖟օյշՅկՏճԴՑգ!?(){}[]@#$%&*+-=<>𝖆́𝖊́𝖎́𝖔́𝖚́𝕬́𝕰́𝕴́𝕺́𝖀́𝖓̃𝕹̃𝖈̧𝕮̧",
  },
  {
    name: "Small Caps",
    chars:
      "ᴀʙᴄᴅᴇғɢʜɪᴊᴋʟᴍɴᴏᴘǫʀsᴛᴜᴠᴡxʏᴢᴀʙᴄᴅᴇғɢʜɪᴊᴋʟᴍɴᴏᴘǫʀsᴛᴜᴠᴡxʏᴢ𝟶𝟷𝟸𝟹𝟺𝟻𝟼𝟽𝟾𝟿!?(){}[]@#$%&*+-=<>ᴀ́ᴇ́ɪ́ᴏ́ᴜ́ᴀ́ᴇ́ɪ́ᴏ́ᴜ́ɴ̃ɴ̃ᴄ̧ᴄ̧",
  },
];
</script>

<template>
  <!-- CDN Font Awesome -->
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.7.2/css/all.min.css"
    integrity="sha512-Evv84Mr4kqVGRNSgIGL/F/aIDqQb7xQ2vcrdIwxfjThSH8CSR7PBEakCr51Ck+w+/U6swU2Im1vVX0SVk9ABhg=="
    crossorigin="anonymous"
    referrerpolicy="no-referrer"
  />

  <main>
    <aside>
      <BotonSwitchTheme @toggle-theme="switchTheme" />
    </aside>

    <h1>WeLoveFonts</h1>

    <input
      maxlength="256"
      type="text"
      v-model="inputUser"
      @focus="onFocus"
      @blur="onBlur"
      :class="{
        'con-texto': inputUser.length > 0,
        'en-foco': isFocused,
        'fuera-foco': !isFocused && inputUser.length > 0,
      }"
      placeholder="Introduce un texto"
    />

    <section>
      <div
        class="box-content"
        v-for="(fuente, i) in FONTS.filter(
          (f) => f.name.toLowerCase() !== 'normal'
        )"
        :key="i"
      >
        <h3 class="fuente-uno">{{ fuente.name }}</h3>

        <p class="fuente-uno">
          {{ toFont(inputUser, fuente.name) }}
        </p>

        <BotonCopiar @click="copiarTexto(toFont(inputUser, fuente.name))" />
      </div>
    </section>
    <footer>
      <div>
        <p>Made by</p>
        <a href="https://github.com/David69redi" target="_blank"
          >David Mejías</a
        >
      </div>
      <div>
        <a target="_blank" href="https://www.instagram.com/itsdavidev?igsh=MXQ1eW9yMHUzeWFyMQ==">
          <i class="fa-brands fa-instagram"></i>
        </a>
      </div>
    </footer>
  </main>
</template>

<style scoped>
main {
  font-family: Arial, Helvetica, sans-serif;
  color: var(--color-text);
  background-color: var(--color-background-body);
  display: flex;
  flex-direction: column;
  width: 100%;
  text-align: center;
  align-items: center;
  align-content: center;
  justify-content: center;

  background-image: radial-gradient(
    var(--second-color-background-body) 0.5px,
    transparent 0.5px
  );
  background-size: 20px 20px;

  aside {
    margin-top: 3rem;
    padding-right: 1rem;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    font-size: 1rem;
  }
  h1 {
    font-family: "Korcy Oblique";
    font-size: 3.8rem;
    margin-top: 20px;
    gap: 30px;
  }

  section {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  input {
  font-size: 1.3rem;
  color: var(--color-text);
  margin-top: 20px;
  margin-bottom: 20px;
  width: 350px;
  height: 40px;
  border: 1.5px dashed var(--color-text); /* borde siempre presente, tipo y grosor fijo */
  border-bottom-width: 2px; /* para que bottom sea más grueso */
  outline: none;
  background: none;
  /* transition: border-color 0.3s ease, border-bottom-width 0.3s ease; */
  border-color: transparent; /* inicial transparente (sin borde visible) */
}

input.en-foco {
  border-bottom-width: 2px;
  border-color: var(--color-text);
  border-bottom: solid; /* cambia solo bottom a sólido */
  border-left: none;
  border-right: none;
  border-top: none;
}

input.fuera-foco {
  border-color: var(--color-text);
  border-style: dashed;
  border-bottom-width: 1.5px;
}

input:not(.con-texto):not(.en-foco) {
  border-color: transparent;
}

}

footer {
  font-size: 1rem;
  /* border: 1px solid blue; */
  width: 100%;
  margin-top: 20px;
  padding: 20px;
  display: flex;
  height: 40px;
  align-items: center;
  justify-content: space-between;
  color: var(--color-text);
  text-decoration: none;
  font-family: monospace;
  padding-bottom: 40px;
  a {
    color: var(--color-text);
    font-family: monospace;
    font-weight: 600;
  }
  i {
    font-size: 2.5rem;
    outline: none;
    text-decoration: none;
    color: var(--color-text);
  }

  div {
    display: flex;
    gap: 10px;
  }
}

.box-content {
  scale: 1.2;
  border: 2px solid var(--color-background-card);
  background-color: var(--color-background-card);
  border-radius: 12px;
  width: 300px;
  gap: 16px;
  display: flex;
  flex-direction: column;
  text-align: left;
  margin: 20px;
  padding: 10px;

  .fuente-uno {
    word-wrap: break-word;
    overflow-wrap: break-word;
    white-space: normal;
    display: block;
    width: 100%;
    max-width: 100%;
    height: auto;
    line-height: 1.4;
    justify-content: center;
  }

  h3 {
    color: var(--color-text);
    font-family: monospace;
  }

  p {
    padding: 2px;
    display: flex;
    align-items: center;
    min-height: 30px;
    border: 1.2px dashed var(--color-text);
  }
}

@media (min-width: 768px) {
  main {
    display: flex;
    h1 {
      font-size: 100px;
    }
    flex-direction: column;
    flex-wrap: wrap;
    justify-content: center;
    text-align: center;

    aside {
      width: 75%;
      scale: 1.3;
    }

    input {
      font-size: 1.5rem;
      color: var(--color-text);
      margin-top: 100px;
      margin-bottom: 20px;
      width: 60%;
      height: 40px;
      border: none;
      border-bottom: 1px solid transparent;
      transition: border-bottom-color 0.3s ease;
      background: none;

      &:focus {
        border-bottom: 1px solid var(--color-text);
      }
    }
  }
  footer {
    /* border: 1px solid blue; */
    width: 100%;
    padding-inline: 60px;
    background: none;
  }

  section {
    margin-top: 12px;
    gap: 50px;
    /* border: 1px solid blue; */

    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .box-content {
    /* border: 2px solid var(--color-background-card); */
    scale: 1.2;
    background-color: var(--color-background-card);
    border-radius: 12px;
    width: 50%;
    gap: 16px;
    display: flex;
    flex-direction: column;

    padding: 10px;
  }
}
</style>
