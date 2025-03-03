<script setup>
import { reactive, ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { useToast } from 'vue-toast-notification'
import 'vue-toast-notification/dist/theme-sugar.css'
import axios from 'axios'

const toast = useToast({
  position: 'top-right'
})

const route = useRoute()
const router = useRouter()

const api_url = 'https://mockapi.io/api/products'
const images = ref([])

onMounted(async () => {
  try {
    const response = await axios.get(api_url)
    const products = response.data

    const uniqueImages = [...new Set(products.map(product => product.bild))]
    images.value = uniqueImages
  } catch (error) {
    console.error('Kunde inte hämta bildadresser', error)
  }
})

const form = reactive({
  artnr: '',
  title: '',
  bild: '',
  beskrivning: '',
  pris: '',
})

const generateId = async () => {
  try {
    const response = await axios.get(api_url)
    const products = response.data
    if (products.length > 0) {
      const ids = products.map(product => parseInt(product.id))
      const maxId = Math.max(...ids)
      return maxId + 1
    } else {
      return 1
    }
  } catch (error) {
    console.error('Det gick inte att generera ID', error)
    return 1
  }
}

const handleSubmit = async () => {
  if (!form.bild) {
    alert('Välj en bild för grafikkortet')
    return
  }

  const productId = await generateId()
  const newCard = {
    id: productId,
    artnr: form.artnr,
    title: form.title,
    bild: form.bild,
    beskrivning: form.beskrivning,
    pris: form.pris,
  }
  try {
    await axios.post(api_url, newCard)
    toast.success('Produkt har lagts till')
    router.push(`/products/${productId}`)
  } catch (error) {
    toast.error('Lagt till produkt misslyckades')
    console.error('Det gick inte att lägga in produkt.', error)
  }
}

const selectImage = (imageUrl) => {
  form.bild = imageUrl
}
</script>

<template>
  <h4 class="fw-bold text-light mb-4 text-center">
    Sälj grafikkort
  </h4>
  <section class="container text-white section-card pt-4 mb-5">
    <form class="row g-3 small" @submit.prevent="handleSubmit">
      <div class="col-md-5">
        <label class="form-label">Annonsens namn:</label>
        <select v-model="form.title" id="title" name="title" class="form-select"
          required>
          <option value="ASUS ROG Strix GeForce RTX 3080">ASUS ROG Strix GeForce
            RTX 3080</option>
          <option value="MSI Radeon RX 6800 XT Gaming X Trio">MSI Radeon RX 6800
            XT Gaming X Trio
          </option>
          <option value="Gigabyte AORUS GeForce RTX 3070 Master">Gigabyte AORUS
            GeForce RTX 3070 Master
          </option>
          <option value="EVGA GeForce RTX 3060 Ti FTW3 Ultra">EVGA GeForce RTX
            3060 Ti FTW3 Ultra</option>
          <option value="Sapphire Nitro+ Radeon RX 6900 XT">Sapphire Nitro+
            Radeon RX 6900 XT</option>
          <option value="ZOTAC Gaming GeForce RTX 3090 Trinity">ZOTAC Gaming
            GeForce RTX 3090 Trinity
          </option>
          <option value="PowerColor Red Devil Radeon RX 6700 XT">PowerColor Red
            Devil Radeon RX 6700 XT
          </option>
          <option value="PNY XLR8 GeForce RTX 3060 Revel Epic-X">PNY XLR8
            GeForce RTX 3060 Revel Epic-X</option>
        </select>
      </div>
      <div class="col-6 col-md-4">
        <label for="artnr" class="form-label">Artikelnummer:</label>
        <select v-model="form.artnr" id="artnr" name="artnr" class="form-select"
          required>
          <option value="RTX3080">RTX3080</option>
          <option value="RX6800">RX6800</option>
          <option value="RTX3070">RTX3070</option>
          <option value="RTX3060">RTX3060</option>
          <option value="RX6900">RX6900</option>
          <option value="RTX3090">RTX3090</option>
          <option value="RX6700XT">RX6700XT</option>
          <option value="RTX3060EPICX">RTX3060EPICX</option>
        </select>
      </div>

      <div class="col-6 col-md-3">
        <label for="pris" class="form-label">Pris:</label>
        <input type="number" v-model="form.pris" id="pris" name="pris"
          class="form-control" required />
      </div>

      <div class="col-12">
        <label for="beskrivning" class="form-label">Välj beskrivning:</label>
        <select v-model="form.beskrivning" id="beskrivning" name="beskrivning"
          class="form-select" required>
          <option
            value="Avancerade grafikkort för både spel och yrkesmässig användning, designade för att hantera krävande grafikuppgifter och ge en oöverträffad prestanda för både gamers och yrkesverksamma som arbetar med grafiskt intensiva applikationer.">
            Avancerade grafikkort för både spel och yrkesmässig användning
          </option>
          <option
            value="Kraftfulla GPU:er för gamers och professionella, utrustade med den senaste tekniken för att leverera exceptionell visuell upplevelse och blixtsnabba renderingstider, idealiska för både underhållning och professionell användning i arbetsmiljöer.">
            Kraftfulla GPU:er för gamers och professionella
          </option>
          <option
            value="Högklassiga grafiklösningar för spel och arbetsstationer, som erbjuder en balans mellan kraft, effektivitet och tillförlitlighet, perfekt för användare som kräver både utmärkt spelprestanda och kapacitet för avancerade professionella uppgifter.">
            Högklassiga grafiklösningar för spel och
            arbetsstationer</option>
          <option
            value="Grafikprocessorer av hög prestanda för både underhållning och arbete, designade för att ge en fängslande spelupplevelse och samtidigt vara kraftfulla nog att hantera professionella arbetsbelastningar som kräver precisionsgrafik.">
            Grafikprocessorer av hög prestanda för både
            underhållning och arbete</option>
          <option
            value="Professionella grafikkort som också är perfekta för gaming, och erbjuder ett brett spektrum av funktioner som är anpassade för både krävande spelare och yrkesutövare som behöver högpresterande grafikkort för sina dagliga arbetsuppgifter.">
            Professionella grafikkort som också är perfekta för
            gaming</option>
          <option
            value="GPU:er designade för både spelentusiaster och yrkesverksamma, utrustade med den senaste grafikteknologin för att leverera exceptionell visuell upplevelse, hög bildkvalitet och snabb rendering för både fritidsaktiviteter och professionellt arbete.">
            GPU:er designade för både spelentusiaster och
            yrkesverksamma</option>
          <option
            value="Top-of-the-line grafikkort för både spelsektorn och professionella användningsområden, som erbjuder bästa prestandan och funktionaliteten för användare som kräver det allra bästa från sina grafikkort, oavsett om det är för spel eller grafiskt krävande projekt.">
            Top-of-the-line grafikkort för både spelsektorn och
            professionella</option>
          <option
            value="Specialiserade grafikkort för både spelare och yrkesutövare, utformade för att möta behoven hos både gamers som söker den ultimata spelupplevelsen och yrkesverksamma som kräver robusta och pålitliga grafikkort för sina kreativa och tekniska arbetsuppgifter.">
            Specialiserade grafikkort för både spelare och
            yrkesutövare</option>
        </select>
      </div>

      <label class="form-label">Välj bild på grafikkort:</label>
      <div class="col-12 mt-2">
        <div
          class="d-flex flex-wrap gap-1 image-gallery mt-2 justify-content-center">
          <div v-for="image in images" :key="image" class="image-option"
            :class="{ 'selected': form.bild === image }"
            @click="selectImage(image)">
            <img :src="image" alt="Grafikkort" class="img-fluid" />
          </div>
        </div>
        <div v-if="!images.length" class="text-center py-3">
          Laddar bilder...
        </div>
      </div>
      <div class="col-12 d-flex justify-content-end mb-5">
        <button type="submit" class="btn btn-primary py-2 px-5 mt-2">Skicka
          in</button>
      </div>
    </form>
  </section>
</template>

<style scoped>
.btn-primary
{
  background-color: #bb81f8;
}

.btn-primary:hover
{
  background-color: #9a67ea;
}

.small
{
  font-size: 1rem;
  width: 1000px;
  max-width: 100%;
  margin: 0 auto;
  display: flex;
  align-items: center;
}

.form-control,
.form-select
{
  padding: 0.25rem 0.5rem;
}

h2
{
  font-size: 1.5rem;
}

#bg
{
  background: #413d46;
}

.image-gallery
{
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.image-option
{
  width: 100px;
  height: 100px;
  border: 2px solid #ccc;
  border-radius: 6px;
  overflow: hidden;
  cursor: pointer;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f8f9fa;
}

.image-option:hover
{
  border-color: #0d6efd;
  transform: scale(1.05);
}

.image-option.selected
{
  border: 3px solid #ffffff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  background: #bb81f8;
}

.image-option img
{
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}

.section-card
{
  background: #1e1e1e;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  border: none;
}
</style>
