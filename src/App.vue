<script setup>
import { computed, ref, watch, onBeforeUnmount } from 'vue'
import {
  MapPin,
  CalendarDays,
  Users,
  Clock,
  Globe,
  Lock,
  ChevronDown,
  Instagram,
  X,
} from 'lucide-vue-next'

import logoAedg from '@/assets/aedg.png'
import logoAeep from '@/assets/aeep.jpeg'
import logoAeg from '@/assets/aeg.JPG'
import logoAei from '@/assets/aei.jpeg'
import logoAete from '@/assets/aete.png'
import logoAgia from '@/assets/agia.png'
import logoAsam from '@/assets/asam.png'
import logoAsoadu from '@/assets/asoadu.jpeg'
import logoCac from '@/assets/cac.png'
import logoDn from '@/assets/dn.png'
import logoInfo from '@/assets/INFO.png'
import logoBiblio from '@/assets/biblio.jpg'

const DAY_COLORS = ['#FF4D1C', '#00B896', '#F5B800', '#D94FD5', '#2563EB', '#16A34A']

const instagramLinks = {
  aeg: 'https://www.instagram.com/ucr.aeg/',
  aete: 'https://www.instagram.com/aete.sg/',
  asoinfo: 'https://www.instagram.com/asoinfo_sg/',
  acep: 'https://www.instagram.com/aecep_ucr/',
  asoadu: 'https://www.instagram.com/asoadu_sg/',
  aei: 'https://www.instagram.com/aei_ucr_sg/',
  asam: 'https://www.instagram.com/asam.ucrsg/',
  aedg: 'https://www.instagram.com/aso_derecho_gte/',
  dn: 'https://www.instagram.com/dn_guanacaste/',
  agia: 'https://www.instagram.com/a.g.i.a/',
  cac: 'https://www.instagram.com/cacsg.ucr/',
  biblioteca: 'https://www.instagram.com/bibliotecasg_ucr/',
}

const organizadores = [
  {
    nombre: 'Asoc. Estudiantes Sede Guanacaste',
    handle: '@ucr.aeg',
    instagram: instagramLinks.aeg,
    logo: logoAeg,
  },
  {
    nombre: 'Asoc. Estudiantes de Turismo',
    handle: '@aete.sg',
    instagram: instagramLinks.aete,
    logo: logoAete,
  },
  {
    nombre: 'Asoc. Estudiantes Informática',
    handle: '@asoinfo_sg',
    instagram: instagramLinks.asoinfo,
    logo: logoInfo,
  },
  {
    nombre: 'Asoc. Estudiantes Educación Primaria',
    handle: '@aecep_ucr',
    instagram: instagramLinks.acep,
    logo: logoAeep,
  },
  {
    nombre: 'Asoc. Estudiantes de Aduanas',
    handle: '@asoadu_sg',
    instagram: instagramLinks.asoadu,
    logo: logoAsoadu,
  },
  {
    nombre: 'Asoc. Estudiantes de Inglés',
    handle: '@aei_ucr_sg',
    instagram: instagramLinks.aei,
    logo: logoAei,
  },
  {
    nombre: 'Asoc. Estudiantes Salud Ambiental',
    handle: '@asam.ucrsg',
    instagram: instagramLinks.asam,
    logo: logoAsam,
  },
  {
    nombre: 'Asoc. Estudiantes de Derecho',
    handle: '@aso_derecho_gte',
    instagram: instagramLinks.aedg,
    logo: logoAedg,
  },
  {
    nombre: 'Asoc. Estudiantes Dirección de Negocios',
    handle: '@dn_guanacaste',
    instagram: instagramLinks.dn,
    logo: logoDn,
  },
  {
    nombre: 'Asoc. Estudiantes Ing. Alimentos',
    handle: '@a.g.i.a',
    instagram: instagramLinks.agia,
    logo: logoAgia,
  },
  {
    nombre: 'Consejo de Asociaciones de Carrera',
    handle: '@cacsg.ucr',
    instagram: instagramLinks.cac,
    logo: logoCac,
  },
  {
    nombre: 'Biblioteca Sede Guanacaste',
    handle: '@bibliotecasg_ucr',
    instagram: instagramLinks.biblioteca,
    logo: logoBiblio,
  },
]

const agenda = [
  {
    nombre: 'Lunes',
    fecha: '20 de Abril',
    actividades: [
      {
        hora: '08:00 a.m. - 12:00 m.d.',
        nombre: 'Voleibol en la plaza',
        lugar: 'Plaza',
        descripcion: 'Actividad recreativa de voleibol para estudiantes.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '09:00 a.m.',
        nombre: 'Quéjese',
        lugar: 'Pasillos 1, 2 y 3 pabellón',
        descripcion: 'Espacio abierto para expresar inquietudes sobre la Sede y proponer mejoras.',
        responsable: 'Asoc. Estudiantes de Inglés',
        publico: 'Abierta a la Comunidad Universitaria',
      },

      {
        hora: '12:00 m.d.',
        nombre: 'Gran Bingo',
        lugar: 'Salón Multiusos',
        descripcion: 'Bingo con muchos premios y sorpresas.',
        responsable: 'Asoc. Estudiantes Ing. Alimentos',
        publico: 'Abierta a Público General',
      },
      {
        hora: '01:00 p.m.',
        nombre: 'Just Dance',
        lugar: 'Biblioteca',
        descripcion: 'Concurso de baile con el videojuego Just Dance.',
        responsable: 'Asoc. Estudiantes de Inglés',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '03:00 p.m. - 05:00 p.m.',
        nombre: 'Cafeteada',
        lugar: 'Edificio AEG',
        descripcion: 'Cafeteada para compartir un momento de convivencia.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '07:00 p.m.',
        nombre: 'Presentación de Baile Popular-Ritmo y Sol',
        lugar: 'Multiusos',
        descripcion: 'Presentación de baile a cargo del grupo Ritmo y Sol.',
        responsable: 'Gestión Cultural Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
    ],
  },
  {
    nombre: 'Martes',
    fecha: '21 de Abril',
    actividades: [
      {
        hora: '09:00 a.m.',
        nombre: 'Spelling Bee',
        lugar: 'Miniauditorio',
        descripcion: 'Concurso de deletreo en inglés con múltiples rondas.',
        responsable: 'Asoc. Estudiantes de Inglés',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '10:00 a.m. - 04:00 p.m.',
        nombre: 'Taller de Totebags',
        lugar: 'Salón Multiusos',
        descripcion: 'Taller de pintura y personalización de bolsas de tela.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '10:00 a.m.',
        nombre: 'Corrige el Código',
        lugar: 'Espacio Virtual',
        descripcion: 'Reto de análisis y depuración de código para Informática.',
        responsable: 'Asoc. Estudiantes Informática',
        publico: 'Limitada a la Población de la Carrera',
      },
      {
        hora: '10:00 a.m.',
        nombre: 'Taller de Lesco',
        lugar: 'Aula 11',
        descripcion: 'Taller introductorio de LESCO (Lengua de Señas Costarricense) dirigido a estudiantes interesados en aprender comunicación básica inclusiva.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '10:00 a.m. - 12:00 m.d.',
        nombre: 'Todas las preguntas inician por…',
        lugar: 'Pasillos del primer, segundo y tercer pabellón',
        descripcion: 'Actividad de análisis',
        responsable: 'Asoc. Estudiantes Informática',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '12:00 p.m.',
        nombre: 'Karaoke',
        lugar: 'Aula 20',
        descripcion: 'Tarde de karaoke para demostrar el talento vocal de los estudiantes.',
        responsable: 'Asoc. Estudiantes de Inglés',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '01:00 p.m. - 05:00 p.m.',
        nombre: 'Día de shows de talentos',
        lugar: 'Salón Multiusos',
        descripcion: 'Presentaciones de talento estudiantil en distintas disciplinas.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
    ],
  },
  {
    nombre: 'Miércoles',
    fecha: '22 de Abril',
    actividades: [
      {
        hora: '08:00 a.m.',
        nombre: 'Cine Bloque A',
        lugar: 'Aula 3',
        descripcion: 'Sesión de cine matutina para empezar el día con buena energía.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '09:00 a.m.',
        nombre: 'Olimpiadas de Debate',
        lugar: 'Sala de Juicios',
        descripcion: 'Concurso de argumentación y oratoria para estudiantes de Derecho.',
        responsable: 'Asoc. Estudiantes de Derecho',
        publico: 'Limitada a la población de la carrera',
      },
      {
        hora: '09:00 a.m. - 12:00 m.d.',
        nombre: 'Juegos recreativos',
        lugar: 'A la par del edificio',
        descripcion: 'Juegos de mesa, ping pong, bádminton y más.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '10:00 a.m.',
        nombre: 'Picnic',
        lugar: 'Nuevas mesitas al costado del comedor',
        descripcion: 'Actividad para compartir al aire libre con compañeros.',
        responsable: 'Asoc. Estudiantes de Inglés',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '12:00 m.d.',
        nombre: 'Cine Bloque B',
        lugar: 'Aula 16',
        descripcion: 'Sesión de cine al mediodía.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '01:00 p.m.',
        nombre: 'Rally DN',
        lugar: 'Cancha de Futbol',
        descripcion: 'Rally temático con pruebas y retos para estudiantes.',
        responsable: 'Asoc. Estudiantes Dirección de Negocios',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '01:00 p.m. - 04:00 p.m.',
        nombre: 'Karaoke',
        lugar: 'Salón Multiusos',
        descripcion: 'Actividad recreativa de canto para estudiantes.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '02:00 p.m.',
        nombre: 'Tarde de Cine',
        lugar: 'Sala de estudio individual - Biblioteca',
        descripcion: 'Tarde de convivio y películas seleccionadas por la carrera.',
        responsable: 'Asoc. Estudiantes Salud Ambiental',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '02:00 p.m.',
        nombre: 'Tarde de Cine',
        lugar: 'Sala de estudio grupal - biblioteca',
        descripcion: 'Tarde de convivio y películas seleccionadas por la carrera.',
        responsable: 'Asoc. Estudiantes de Derecho',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '02:00 p.m.',
        nombre: 'Tarde de Juegos femeninos',
        lugar: 'Biblioteca',
        descripcion: 'Tarde de convivio y películas seleccionadas por la carrera.',
        responsable: 'Biblioteca Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '03:00 p.m.',
        nombre: 'Tardeada de Artes',
        lugar: 'Salón Multiusos',
        descripcion: 'Espacio creativo, se realizarán una tarde de arte con los estudiantes de la carrera',
        responsable: 'Asoc. Estudiantes de Turismo',
        publico: 'Limitada a la Población de la Carrera',
      },
      {
        hora: '04:00 p.m.',
        nombre: 'Cine Bloque C',
        lugar: 'Aula 19',
        descripcion: 'Cierre vespertino del miércoles de cine.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '04:00 p.m.',
        nombre: 'Presentación del Grupo Ensamble Musical',
        lugar: 'Multiusos',
        descripcion: 'Presentación musical a cargo del grupo Ensamble Musical de la Sede Guanacaste.',
        responsable: 'Gestión Cultural Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
    ],
  },
  {
    nombre: 'Jueves',
    fecha: '23 de Abril',
    actividades: [
      {
        hora: '09:00 a.m.',
        nombre: 'Búsqueda del Tesoro',
        lugar: 'Diferentes áreas',
        descripcion: 'Búsqueda de pistas por todo el campus para Turismo.',
        responsable: 'Asoc. Estudiantes de Turismo',
        publico: 'Limitada a la Población de la Carrera',
      },
      {
        hora: '09:00 a.m. - 12:00 a.m.',
        nombre: 'Bingo Literario',
        lugar: 'Diferentes áreas',
        descripcion: '¡¡Donde habrá mucha diversión, premios y lectura!!',
        responsable: 'Biblioteca Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '10:00 a.m.',
        nombre: 'Torneo Play Station',
        lugar: 'Biblioteca',
        descripcion: 'Torneo de videojuegos en consola abierto a toda la comunidad.',
        responsable: 'Asoc. Estudiantes de Inglés',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '01:00 p.m.',
        nombre: 'Convivio',
        lugar: 'Rancho de Deportivas',
        descripcion: 'Convivio exclusivo para la carrera de Salud Ambiental.',
        responsable: 'Asoc. Estudiantes Salud Ambiental',
        publico: 'Limitada a la Población de la Carrera',
      },
      {
        hora: '01:00 p.m. - 03:00 p.m.',
        nombre: 'Tardeada de arte',
        lugar: 'Salón Multiusos (por confirmar)',
        descripcion: 'Concurso de pintura sobre lienzo con reconocimiento para las 3 mejores obras. Pendiente por disponibilidad de multiusos y confirmación de profesora de arte.',
        responsable: 'Asoc. Estudiantes de Turismo',
        publico: 'Limitada a la Población de la Carrera',
      },
      {
        hora: '01:00 p.m. - 04:00 p.m.',
        nombre: 'TALLER DE DIBUJO CREATIVO',
        lugar: 'Audiovisuales 2 (dentro de la Biblioteca)',
        descripcion: 'el Club de Arte, organiza este taller de dibujo creativo, pensado para que te diviertas creando durante semana U. Se realizarán diversos ejercicios de dibujo en un espacio seguro, con el objetivo de fomentar la creatividad, explorar la imaginación y superar bloqueos creativos.',
        responsable: 'Vida Estudiantil',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '02:00 p.m. - 04:00 p.m.',
        nombre: 'Charlas con expositores invitados',
        lugar: 'Auditorio',
        descripcion: 'Expositores invitados expondrán temas de interés relacionados con el turismo y los recursos naturales o turísticos. Temas específicos por confirmar.',
        responsable: 'Asoc. Estudiantes de Turismo',
        publico: 'Limitada a la Población de la Carrera',
      },
      {
        hora: '02:00 p.m.',
        nombre: '¿Quién quiere ser jurista?',
        lugar: 'Sala de Juicios',
        descripcion: 'Concurso de conocimientos jurídicos al estilo de programa de TV.',
        responsable: 'Asoc. Estudiantes de Derecho',
        publico: 'Limitada a la Población de la Carrera',
      },
      {
        hora: '04:00 p.m.',
        nombre: 'Presentación de Teatro-Catarsis',
        lugar: 'Multiusos',
        descripcion: 'Presentación teatral a cargo del grupo Teatro-Catarsis de la Sede Guanacaste.',
        responsable: 'Gestión Cultural Sede Guanacaste',
        publico: 'xc',
      },
      {
        hora: '05:00 p.m.',
        nombre: 'Presentación de Baile Folclórico-Grupo Huanacaxtle',
        lugar: 'Multiusos',
        descripcion: 'Presentación de baile folclórico a cargo del grupo Huanacaxtle de la Sede Guanacaste.',
        responsable: 'Gestión Cultural Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
    ],
  },
  {
    nombre: 'Viernes',
    fecha: '24 de Abril',
    actividades: [
      {
        hora: '09:00 a.m. - 04:00 p.m.',
        nombre: 'Búsqueda del tesoro',
        lugar: 'Distintos sitios de la sede',
        descripcion: 'Se brindarán pistas sobre objetos escondidos alrededor de la Sede y quien los encuentre será la persona ganadora de dicho objeto.',
        responsable: 'Asoc. Estudiantes de Turismo',
        publico: 'Limitada a la Población de la Carrera',
      },
      {
        hora: '09:00 a.m. - 12:00 p.m.',
        nombre: 'Rally de obstáculos',
        lugar: 'Cancha de futbol de la sede',
        descripcion: 'Desarrollo de actividades en parejas o equipos con obstáculos y estaciones.',
        responsable: 'Asoc. Estudiantes de Turismo',
        publico: 'Limitada a la Población de la Carrera',
      },
      {
        hora: '10:00 a.m. - 01:00 p.m.',
        nombre: 'Picnic"',
        lugar: 'Costado Edificio AEG',
        descripcion: 'Picnic al aire libre para compartir con compañeros y compañeras.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '11:00 a.m.',
        nombre: 'Presentación de la Cimarrona de la sede Guanacaste',
        lugar: 'Multiusos',
        descripcion: 'Presentación de la Cimarrona, a cargo de los estudiantes de la Sede Guanacaste.',
        responsable: 'Gestión Cultural Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '12:00 p.m.',
        nombre: 'Karaoke Clásico',
        lugar: 'Salón Multiusos',
        descripcion: 'Tarde de karaoke con los éxitos de todos los tiempos.',
        responsable: 'Asoc. Estudiantes Sede Guanacaste',
        publico: 'Abierta a la Comunidad Universitaria',
      },
      {
        hora: '01:00 p.m.',
        nombre: 'Búsqueda del Tesoro',
        lugar: 'Zonas verdes',
        descripcion: 'Actividad de búsqueda de objetos escondidos en el campus.',
        responsable: 'Asoc. Estudiantes Educación Primaria',
        publico: 'Limitada a la población de la carrera',
      },
      {
        hora: '02:00 p.m.',
        nombre: 'Charla con Expertos',
        lugar: 'Miniauditorio',
        descripcion: 'Charlas orientadas a reforzar áreas clave de la carrera.',
        responsable: 'Asoc. Estudiantes de Turismo',
        publico: 'Limitada a la Población de la Carrera',
      },
      {
        hora: 'Todo el día',
        nombre: 'Olimpiadas Ambientales',
        lugar: 'Instalaciones deportivas',
        descripcion: 'Jornada completa de actividades recreativas para Salud Ambiental.',
        responsable: 'Asoc. Estudiantes Salud Ambiental',
        publico: 'Limitada a la Población de la Carrera',
      },
    ],
  },
  {
    nombre: 'Sábado',
    fecha: '25 de Abril',
    actividades: [
      {
        hora: '01:00 p.m.',
        nombre: 'Karaoke de Cierre',
        lugar: 'Miniauditorio',
        descripcion: 'Tarde de karaoke para despedir la Semana U 2026 con todo.',
        responsable: 'Asoc. Estudiantes de Aduanas',
        publico: 'Limitada a Aduanas',
      },
    ],
  },
]

const selectedEvent = ref(null)
const selectedEventDay = ref(null)
const selectedEventColor = ref(DAY_COLORS[0])

function normalizeText(text = '') {
  return text
    .toLowerCase()
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')
}

function isOpen(ev) {
  return normalizeText(ev.publico).includes('abierta')
}

function getInstagram(ev) {
  const responsable = normalizeText(ev?.responsable || '')

  if (responsable.includes('biblioteca')) return instagramLinks.biblioteca
  if (responsable.includes('sede guanacaste')) return instagramLinks.aeg
  if (responsable.includes('turismo')) return instagramLinks.aete
  if (responsable.includes('informatica')) return instagramLinks.asoinfo
  if (responsable.includes('educacion primaria')) return instagramLinks.acep
  if (responsable.includes('aduanas')) return instagramLinks.asoadu
  if (responsable.includes('ingles')) return instagramLinks.aei
  if (responsable.includes('salud ambiental')) return instagramLinks.asam
  if (responsable.includes('derecho')) return instagramLinks.aedg
  if (responsable.includes('direccion de negocios')) return instagramLinks.dn
  if (responsable.includes('direccion de empresas')) return instagramLinks.dn
  if (responsable.includes('ing. alimentos')) return instagramLinks.agia
  if (responsable.includes('ingenieria de alimentos')) return instagramLinks.agia
  if (responsable.includes('consejo de asociaciones')) return instagramLinks.cac
  

  return ''
}

function getInstagramHandle(ev) {
  const url = getInstagram(ev)
  if (!url) return 'No disponible'
  const clean = url.replace('https://www.instagram.com/', '').replaceAll('/', '')
  return `@${clean}`
}

function openEvent(ev, dayObj, index) {
  selectedEvent.value = ev
  selectedEventDay.value = dayObj
  selectedEventColor.value = DAY_COLORS[index]
}

function closeEvent() {
  selectedEvent.value = null
  selectedEventDay.value = null
}

function handleKeydown(e) {
  if (e.key === 'Escape' && selectedEvent.value) closeEvent()
}

watch(selectedEvent, (value) => {
  document.body.style.overflow = value ? 'hidden' : ''
})

window.addEventListener('keydown', handleKeydown)

onBeforeUnmount(() => {
  document.body.style.overflow = ''
  window.removeEventListener('keydown', handleKeydown)
})

const totalActividades = computed(() =>
  agenda.reduce((s, d) => s + d.actividades.length, 0),
)

const totalAbiertas = computed(() =>
  agenda.reduce((s, d) => s + d.actividades.filter(isOpen).length, 0),
)

const totalLimitadas = computed(() =>
  totalActividades.value - totalAbiertas.value,
)
</script>

<template>
  <div class="su-app">
    <header class="hero">
      <span class="hero__bg-letter" aria-hidden="true">U</span>

      <div class="shape shape--circle-lg"></div>
      <div class="shape shape--circle-sm"></div>
      <div class="shape shape--square"></div>
      <div class="shape shape--pill"></div>
      <div class="shape shape--ring"></div>

      <div class="hero__content">
        <div class="hero__eyebrow">
          <div class="eyebrow__line"></div>
          <span>UCR · Sede Guanacaste</span>
          <div class="eyebrow__line"></div>
        </div>

        <h1 class="hero__heading">
          <span class="heading__semana">SEMANA</span>
          <span class="heading__u">U</span>
          <span class="heading__year">'26</span>
        </h1>

        <div class="hero__meta">
          <div class="meta-chip meta-chip--dark">
            <CalendarDays :size="14" />
            20 – 25 de Abril
          </div>
          <div class="meta-chip meta-chip--orange">
            <MapPin :size="14" />
            Guanacaste
          </div>
          <div class="meta-chip meta-chip--teal">
            <Users :size="14" />
            Comunidad Universitaria
          </div>
        </div>

        <p class="hero__desc">
          Seis días de actividades, cultura, deporte y convivencia para toda la comunidad de la Sede.
        </p>

        <a href="#agenda" class="hero__scroll">
          <ChevronDown :size="22" />
          <span>Ver programa completo</span>
        </a>
      </div>

      <div class="hero__stats">
        <div class="stat">
          <span class="stat__n">6</span>
          <span class="stat__l">Días</span>
        </div>
        <div class="stat__div"></div>
        <div class="stat">
          <span class="stat__n">{{ totalActividades }}</span>
          <span class="stat__l">Actividades</span>
        </div>
        <div class="stat__div"></div>
        <div class="stat">
          <span class="stat__n">{{ totalAbiertas }}</span>
          <span class="stat__l">Abiertas</span>
        </div>
        <div class="stat__div"></div>
        <div class="stat">
          <span class="stat__n">{{ totalLimitadas }}</span>
          <span class="stat__l">Por carrera</span>
        </div>
      </div>
    </header>

    <section class="access-legend">
      <div class="access-legend__inner">
        <div class="access-legend__item access-legend__item--open">
          <div class="access-legend__icon">
            <Globe :size="18" />
          </div>
          <div class="access-legend__text">
            <span class="access-legend__label">Para todo público</span>
            <span class="access-legend__sub">Actividades abiertas a la comunidad universitaria o público general</span>
          </div>
        </div>

        <div class="access-legend__item access-legend__item--limited">
          <div class="access-legend__icon">
            <Lock :size="18" />
          </div>
          <div class="access-legend__text">
            <span class="access-legend__label">Limitado para carrera</span>
            <span class="access-legend__sub">Actividades exclusivas para estudiantes de una carrera específica</span>
          </div>
        </div>
      </div>
    </section>

    <main id="agenda" class="agenda">
      <section
        v-for="(dia, di) in agenda"
        :key="dia.nombre"
        class="day-section"
      >
        <div class="day-band" :style="{ '--band': DAY_COLORS[di] }">
          <div class="day-band__left">
            <span class="day-band__index">{{ String(di + 1).padStart(2, '0') }}</span>
            <div>
              <h2 class="day-band__name">{{ dia.nombre }}</h2>
              <p class="day-band__date">{{ dia.fecha }}</p>
            </div>
          </div>
          <div class="day-band__pill">{{ dia.actividades.length }} actividades</div>
        </div>

        <div class="cards-grid">
          <article
            v-for="(ev, ei) in dia.actividades"
            :key="`${dia.nombre}-${ei}-${ev.nombre}`"
            :class="['ev-card', `ev-card--skin${(di * 7 + ei) % 5}`]"
            role="button"
            tabindex="0"
            @click="openEvent(ev, dia, di)"
            @keydown.enter="openEvent(ev, dia, di)"
            @keydown.space.prevent="openEvent(ev, dia, di)"
          >
            <div
              class="ev-card__dot"
              :style="{ background: DAY_COLORS[di] }"
            ></div>

            <div class="ev-card__time">
              <Clock :size="12" />
              {{ ev.hora }}
            </div>

            <h3 class="ev-card__name">{{ ev.nombre }}</h3>

            <p class="ev-card__desc">{{ ev.descripcion }}</p>

            <div class="ev-card__meta">
              <div class="ev-card__row">
                <MapPin :size="13" class="row-icon" />
                <span>{{ ev.lugar }}</span>
              </div>
              <div class="ev-card__row ev-card__row--org">
                <Users :size="13" class="row-icon" />
                <span>{{ ev.responsable }}</span>
              </div>
            </div>

            <div class="ev-card__footer">
              <div
                class="ev-card__status"
                :class="isOpen(ev) ? 'ev-card__status--open' : 'ev-card__status--ltd'"
              >
                <component :is="isOpen(ev) ? Globe : Lock" :size="12" />
                {{ isOpen(ev) ? 'Abierta' : 'Limitada' }}
              </div>

              <span class="ev-card__more">Ver detalle</span>
            </div>

            <div
              class="ev-card__shape"
              :style="{ background: DAY_COLORS[di] }"
            ></div>
          </article>
        </div>
      </section>
    </main>

    <section class="organizers-band">
      <div class="organizers-band__inner">
        <div class="organizers-band__header">
          <h2>Organizan</h2>
          <p>
            Agrupaciones estudiantiles que hacen posible la Semana U 2026 en la Sede Guanacaste.
          </p>
        </div>

        <div class="organizers-marquee">
          <div class="organizers-marquee__fade organizers-marquee__fade--left"></div>
          <div class="organizers-marquee__fade organizers-marquee__fade--right"></div>

          <div class="organizers-marquee__track">
            <a
              v-for="(org, index) in [...organizadores, ...organizadores]"
              :key="`${org.nombre}-${index}`"
              class="organizer-card"
              :href="org.instagram"
              target="_blank"
              rel="noopener noreferrer"
            >
              <div class="organizer-card__logo">
                <img :src="org.logo" :alt="org.nombre" />
              </div>
              <span class="organizer-card__name">{{ org.nombre }}</span>
              <span class="organizer-card__ig">{{ org.handle }}</span>
            </a>
          </div>
        </div>
      </div>
    </section>

    <transition name="modal-fade">
      <div
        v-if="selectedEvent"
        class="event-modal-overlay"
        @click.self="closeEvent"
      >
        <transition name="modal-scale">
          <div v-if="selectedEvent" class="event-modal">
            <button class="event-modal__close" @click="closeEvent" aria-label="Cerrar">
              <X :size="20" />
            </button>

            <div class="event-modal__header" :style="{ '--accent': selectedEventColor }">
              <div class="event-modal__day">
                <span class="event-modal__day-name">{{ selectedEventDay?.nombre }}</span>
                <span class="event-modal__day-date">{{ selectedEventDay?.fecha }}</span>
              </div>

              <div
                class="event-modal__status"
                :class="isOpen(selectedEvent) ? 'event-modal__status--open' : 'event-modal__status--ltd'"
              >
                <component :is="isOpen(selectedEvent) ? Globe : Lock" :size="14" />
                {{ isOpen(selectedEvent) ? 'Abierta al público' : 'Actividad limitada' }}
              </div>
            </div>

            <div class="event-modal__content">
              <div class="event-modal__main">
                <div class="event-modal__time">
                  <Clock :size="15" />
                  {{ selectedEvent.hora }}
                </div>

                <h3 class="event-modal__title">{{ selectedEvent.nombre }}</h3>

                <p class="event-modal__desc">{{ selectedEvent.descripcion }}</p>

                <div class="event-info-grid">
                  <div class="event-info-card">
                    <span class="event-info-card__label">Lugar</span>
                    <div class="event-info-card__value">
                      <MapPin :size="15" />
                      <span>{{ selectedEvent.lugar }}</span>
                    </div>
                  </div>

                  <div class="event-info-card">
                    <span class="event-info-card__label">Organiza</span>
                    <div class="event-info-card__value">
                      <Users :size="15" />
                      <span>{{ selectedEvent.responsable }}</span>
                    </div>
                  </div>

                  <div class="event-info-card">
                    <span class="event-info-card__label">Público</span>
                    <div class="event-info-card__value">
                      <component :is="isOpen(selectedEvent) ? Globe : Lock" :size="15" />
                      <span>{{ selectedEvent.publico }}</span>
                    </div>
                  </div>

                  <div class="event-info-card" v-if="getInstagram(selectedEvent)">
                    <span class="event-info-card__label">Instagram</span>
                    <div class="event-info-card__value event-info-card__value--link">
                      <Instagram :size="15" />
                      <a
                        :href="getInstagram(selectedEvent)"
                        target="_blank"
                        rel="noopener noreferrer"
                      >
                        {{ getInstagramHandle(selectedEvent) }}
                      </a>
                    </div>
                  </div>
                </div>
              </div>

              <aside class="event-modal__aside">
                <div class="event-aside-card">
                  <span class="event-aside-card__eyebrow">Asociación</span>
                  <h4 class="event-aside-card__title">{{ selectedEvent.responsable }}</h4>
                  <p class="event-aside-card__text">
                    Consulta más información de esta actividad desde la cuenta oficial de la asociación organizadora.
                  </p>

                  <a
                    v-if="getInstagram(selectedEvent)"
                    :href="getInstagram(selectedEvent)"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="event-aside-card__button"
                  >
                    <Instagram :size="16" />
                    Ver Instagram
                  </a>

                  <div v-else class="event-aside-card__empty">
                    Instagram no disponible
                  </div>
                </div>
              </aside>
            </div>
          </div>
        </transition>
      </div>
    </transition>

    <footer class="su-footer">
      <div class="su-footer__brand">
        <span class="footer-mark">SU<em>'26</em></span>
        <p class="su-footer__tag">
          Universidad de Costa Rica · Sede Guanacaste · 20–25 de abril 2026
        </p>
      </div>

      <div class="su-footer__credits">
        <span>Página hecha por</span>
        <a
          href="https://www.instagram.com/oscar_herraa13/"
          target="_blank"
          rel="noopener noreferrer"
        >
          Oscar Herra Abarca - Est. Informática Empresarial
        </a>
      </div>
    </footer>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@900&family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap');

:root {
  --cream: #fdf8f0;
  --ink: #1a1108;
  --muted: #7a6e62;
  --border: #e8dfd0;
  --r: #ff4d1c;
  --g: #00b896;
  --y: #f5b800;
  --p: #d94fd5;
  --b: #2563eb;
  --gr: #16a34a;
}

.su-app {
  background: #fdf8f0;
  color: #1a1108;
  font-family: 'Plus Jakarta Sans', sans-serif;
  min-height: 100vh;
  overflow-x: hidden;
}

/* HERO */
.hero {
  position: relative;
  min-height: 100svh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  overflow: hidden;
  background: #fdf8f0;
  border-bottom: 2.5px solid #1a1108;
}

.hero__bg-letter {
  position: absolute;
  font-family: 'Playfair Display', serif;
  font-size: clamp(300px, 52vw, 660px);
  font-weight: 900;
  line-height: 1;
  color: rgba(255, 77, 28, 0.05);
  -webkit-text-stroke: 2px rgba(255, 77, 28, 0.12);
  pointer-events: none;
  user-select: none;
  right: -4vw;
  top: 50%;
  transform: translateY(-50%);
  z-index: 0;
}

.shape {
  position: absolute;
  pointer-events: none;
  z-index: 0;
}

.shape--circle-lg {
  width: 260px;
  height: 260px;
  border-radius: 50%;
  background: var(--r);
  top: -80px;
  right: -80px;
  opacity: 0.9;
  animation: floatShape 8s ease-in-out infinite alternate;
}

.shape--circle-sm {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background: var(--y);
  bottom: 18%;
  left: 5%;
  animation: floatShape 6s ease-in-out infinite alternate-reverse;
}

.shape--square {
  width: 80px;
  height: 80px;
  border-radius: 14px;
  background: var(--g);
  bottom: 28%;
  right: 8%;
  transform: rotate(22deg);
  animation: floatShapeFixed 7s ease-in-out infinite alternate;
}

.shape--pill {
  width: 160px;
  height: 50px;
  border-radius: 100px;
  background: var(--b);
  top: 18%;
  left: 4%;
  transform: rotate(-14deg);
  opacity: 0.85;
  animation: floatPill 9s ease-in-out infinite alternate-reverse;
}

.shape--ring {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  border: 14px solid var(--p);
  background: transparent;
  top: 52%;
  left: 48%;
  animation: floatShape 10s ease-in-out infinite alternate;
}

@keyframes floatShape {
  from { transform: translateY(0) rotate(0deg); }
  to { transform: translateY(-18px) rotate(8deg); }
}

@keyframes floatShapeFixed {
  from { transform: rotate(22deg) translateY(0); }
  to { transform: rotate(22deg) translateY(-14px); }
}

@keyframes floatPill {
  from { transform: rotate(-14deg) translateY(0); }
  to { transform: rotate(-14deg) translateY(-14px); }
}

.hero__content {
  position: relative;
  z-index: 1;
  max-width: 1200px;
  margin: 0 auto;
  padding: 6rem 2rem 2rem;
  width: 100%;
}

.hero__eyebrow {
  display: flex;
  align-items: center;
  gap: 16px;
  font-size: 0.9rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #7a6e62;
  margin-bottom: 2rem;
}

.eyebrow__line {
  flex: 1;
  max-width: 56px;
  height: 1.5px;
  background: #7a6e62;
}

.hero__heading {
  display: flex;
  align-items: baseline;
  gap: 0.06em;
  flex-wrap: wrap;
  margin-bottom: 2rem;
  line-height: 0.9;
}

.heading__semana {
  font-family: 'Playfair Display', serif;
  font-weight: 900;
  font-size: clamp(3.5rem, 11vw, 9rem);
  color: #1a1108;
  letter-spacing: -0.02em;
}

.heading__u {
  font-family: 'Playfair Display', serif;
  font-weight: 900;
  font-size: clamp(4.2rem, 12.5vw, 10.2rem);
  color: var(--r);
  letter-spacing: -0.015em;
  margin-left: 0.08em;
  margin-right: 0.04em;
  line-height: 0.88;
}

.heading__year {
  font-family: 'Playfair Display', serif;
  font-weight: 900;
  font-size: clamp(2rem, 5vw, 4rem);
  color: #1a1108;
  letter-spacing: -0.01em;
  align-self: flex-end;
  padding-bottom: 0.1em;
  opacity: 0.35;
}

.hero__meta {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 1.6rem;
}

.meta-chip {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  font-size: 0.78rem;
  font-weight: 700;
  padding: 8px 16px;
  border-radius: 100px;
  border: 2px solid transparent;
}

.meta-chip--dark { background: #1a1108; color: #fdf8f0; }
.meta-chip--orange { background: var(--r); color: #fff; }
.meta-chip--teal { background: var(--g); color: #fff; }

.hero__desc {
  max-width: 560px;
  font-size: 1rem;
  font-weight: 300;
  line-height: 1.7;
  color: #7a6e62;
  margin-bottom: 2.4rem;
}

.hero__scroll {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: 0.82rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #7a6e62;
  text-decoration: none;
  animation: bounce 2.4s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(6px); }
}

.hero__stats {
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0;
  margin-top: 3rem;
  background: #1a1108;
  padding: 1.6rem 2rem;
  flex-wrap: wrap;
}

.stat {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2px;
  padding: 0 2.5rem;
}

.stat__n {
  font-family: 'Playfair Display', serif;
  font-size: 2.6rem;
  font-weight: 900;
  color: #fdf8f0;
  line-height: 1;
}

.stat__l {
  font-size: 0.65rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.12em;
  color: rgba(253, 248, 240, 0.4);
}

.stat__div {
  width: 1px;
  height: 40px;
  background: rgba(253, 248, 240, 0.15);
}

/* LEGEND */
.access-legend {
  width: 100%;
  padding: 1.4rem 0 0.4rem;
  background: #fdf8f0;
}

.access-legend__inner {
  max-width: 1280px;
  margin: 0 auto;
  padding: 0 1.5rem;
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 16px;
}

.access-legend__item {
  display: flex;
  align-items: flex-start;
  gap: 14px;
  border-radius: 22px;
  padding: 1rem 1.1rem;
  border: 1.5px solid #e8dfd0;
  background: rgba(255, 255, 255, 0.8);
  box-shadow: 0 10px 24px rgba(26, 17, 8, 0.04);
}

.access-legend__item--open {
  background: linear-gradient(180deg, #f5fffb 0%, #ffffff 100%);
}

.access-legend__item--limited {
  background: linear-gradient(180deg, #fff8fe 0%, #ffffff 100%);
}

.access-legend__icon {
  width: 42px;
  height: 42px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.access-legend__item--open .access-legend__icon {
  background: #e6fbf5;
  color: #00875f;
  border: 1.5px solid #b3f0de;
}

.access-legend__item--limited .access-legend__icon {
  background: #fdf0fd;
  color: #9c2d9c;
  border: 1.5px solid #f0bdef;
}

.access-legend__text {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.access-legend__label {
  font-size: 0.95rem;
  font-weight: 800;
  color: #1a1108;
}

.access-legend__sub {
  font-size: 0.83rem;
  line-height: 1.5;
  color: #7a6e62;
}

/* AGENDA */
.agenda {
  max-width: 1280px;
  margin: 0 auto;
  padding: 0 1.5rem 6rem;
}

.day-section {
  margin-top: 3rem;
}

.day-band {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.6rem 2rem;
  border-radius: 20px;
  background: var(--band, #ff4d1c);
  margin-bottom: 1.8rem;
  flex-wrap: wrap;
  gap: 12px;
}

.day-band__left {
  display: flex;
  align-items: center;
  gap: 18px;
}

.day-band__index {
  font-family: 'Playfair Display', serif;
  font-size: 3rem;
  font-weight: 900;
  color: rgba(255, 255, 255, 0.25);
  line-height: 1;
}

.day-band__name {
  font-family: 'Playfair Display', serif;
  font-size: clamp(1.8rem, 5vw, 3rem);
  font-weight: 900;
  color: #fff;
  line-height: 1;
  letter-spacing: -0.01em;
}

.day-band__date {
  font-size: 0.78rem;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.78);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  margin-top: 3px;
}

.day-band__pill {
  background: rgba(255, 255, 255, 0.2);
  color: #fff;
  font-size: 0.72rem;
  font-weight: 800;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  padding: 6px 16px;
  border-radius: 100px;
  border: 1.5px solid rgba(255, 255, 255, 0.35);
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(285px, 1fr));
  gap: 18px;
}

.ev-card {
  position: relative;
  background: #fff;
  border: 2.5px solid #e8dfd0;
  border-radius: 20px;
  padding: 22px 20px 18px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
  cursor: pointer;
  min-height: 355px;
  height: 100%;
}

.ev-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 20px 50px rgba(26, 17, 8, 0.1);
  border-color: #1a1108;
}

.ev-card--skin0 { background: #ffffff; }
.ev-card--skin1 { background: #fffbf0; }
.ev-card--skin2 { background: #f0fdf8; }
.ev-card--skin3 { background: #fff5fd; }
.ev-card--skin4 { background: #f0f5ff; }

.ev-card__dot {
  position: absolute;
  top: 18px;
  right: 18px;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  opacity: 0.85;
}

.ev-card__shape {
  position: absolute;
  width: 90px;
  height: 90px;
  border-radius: 50%;
  bottom: -36px;
  right: -36px;
  opacity: 0.07;
  pointer-events: none;
  transition: opacity 0.2s;
}

.ev-card:hover .ev-card__shape { opacity: 0.14; }

.ev-card__time {
  display: inline-flex;
  align-items: center;
  gap: 5px;
  font-size: 0.7rem;
  font-weight: 800;
  color: #7a6e62;
  background: #f4efe8;
  padding: 4px 10px;
  border-radius: 100px;
  align-self: flex-start;
  margin-bottom: 12px;
  margin-top: 2px;
  letter-spacing: 0.03em;
}

.ev-card__name {
  font-family: 'Playfair Display', serif;
  font-weight: 900;
  font-size: 1.3rem;
  color: #1a1108;
  line-height: 1.15;
  margin-bottom: 8px;
  letter-spacing: -0.01em;
  padding-right: 20px;
}

.ev-card__desc {
  font-size: 0.84rem;
  font-weight: 300;
  color: #7a6e62;
  line-height: 1.6;
  flex-grow: 1;
  margin-bottom: 16px;
  min-height: 84px;
}

.ev-card__meta {
  border-top: 1.5px solid #e8dfd0;
  padding-top: 12px;
  display: flex;
  flex-direction: column;
  gap: 6px;
  margin-bottom: 14px;
  min-height: 72px;
}

.ev-card__row {
  display: flex;
  align-items: flex-start;
  gap: 7px;
  font-size: 0.78rem;
  font-weight: 500;
  color: #5c5248;
  line-height: 1.4;
}

.ev-card__row--org {
  color: #a09488;
  font-weight: 500;
}

.row-icon {
  flex-shrink: 0;
  margin-top: 2px;
}

.ev-card__footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  margin-top: auto;
}

.ev-card__status {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  align-self: flex-start;
  font-size: 0.65rem;
  font-weight: 800;
  letter-spacing: 0.07em;
  text-transform: uppercase;
  padding: 5px 12px;
  border-radius: 100px;
}

.ev-card__status--open {
  background: #e6fbf5;
  color: #00875f;
  border: 1.5px solid #b3f0de;
}

.ev-card__status--ltd {
  background: #fdf0fd;
  color: #9c2d9c;
  border: 1.5px solid #f0bdef;
}

.ev-card__more {
  font-size: 0.7rem;
  font-weight: 800;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #a09488;
}

/* ORGANIZERS BAND */
.organizers-band {
  width: 100%;
  padding: 2.5rem 0 5rem;
  background: linear-gradient(180deg, #faf4ea 0%, #f5ede1 100%);
  border-top: 1.5px solid #e8dfd0;
  border-bottom: 1.5px solid #e8dfd0;
  overflow: hidden;
}

.organizers-band__inner {
  width: 100%;
}

.organizers-band__header {
  max-width: 1280px;
  margin: 0 auto 1.8rem;
  padding: 0 1.5rem;
}

.organizers-band__header h2 {
  font-family: 'Playfair Display', serif;
  font-size: clamp(2.4rem, 4.5vw, 4.5rem);
  line-height: 0.95;
  color: #1a1108;
  margin-bottom: 0.7rem;
}

.organizers-band__header p {
  max-width: 760px;
  font-size: 1.04rem;
  color: #7a6e62;
  line-height: 1.7;
}

.organizers-marquee {
  position: relative;
  width: 100%;
  overflow: hidden;
}

.organizers-marquee__fade {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 90px;
  z-index: 2;
  pointer-events: none;
}

.organizers-marquee__fade--left {
  left: 0;
  background: linear-gradient(to right, #f5ede1 0%, rgba(245, 237, 225, 0) 100%);
}

.organizers-marquee__fade--right {
  right: 0;
  background: linear-gradient(to left, #f5ede1 0%, rgba(245, 237, 225, 0) 100%);
}

.organizers-marquee__track {
  display: flex;
  align-items: stretch;
  gap: 18px;
  width: max-content;
  padding: 0 1.5rem;
  animation: organizers-scroll 35s linear infinite;
}

.organizers-marquee:hover .organizers-marquee__track {
  animation-play-state: paused;
}

@keyframes organizers-scroll {
  from { transform: translateX(0); }
  to { transform: translateX(-50%); }
}

.organizer-card {
  width: 220px;
  min-width: 220px;
  max-width: 220px;
  border-radius: 24px;
  background: rgba(255, 255, 255, 0.92);
  border: 2px solid #e8dfd0;
  padding: 1.3rem 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  gap: 0.85rem;
  box-shadow: 0 12px 26px rgba(26, 17, 8, 0.05);
  text-decoration: none;
  color: inherit;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  min-height: 250px;
  flex-shrink: 0;
}

.organizer-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 34px rgba(26, 17, 8, 0.08);
}

.organizer-card__logo {
  width: 108px;
  height: 108px;
  border-radius: 22px;
  background: #fff;
  border: 2px solid #e8dfd0;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  padding: 10px;
}

.organizer-card__logo img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.organizer-card__name {
  text-align: center;
  font-size: 0.92rem;
  font-weight: 700;
  color: #40362e;
  line-height: 1.35;
  min-height: 2.7em;
  display: flex;
  align-items: center;
  justify-content: center;
}

.organizer-card__ig {
  font-size: 0.82rem;
  font-weight: 800;
  color: #ff4d1c;
  text-align: center;
  word-break: break-word;
}

/* MODAL */
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.22s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}

.modal-scale-enter-active,
.modal-scale-leave-active {
  transition: transform 0.25s ease, opacity 0.25s ease;
}

.modal-scale-enter-from,
.modal-scale-leave-to {
  transform: scale(0.98);
  opacity: 0;
}

.event-modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(26, 17, 8, 0.55);
  z-index: 999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1.25rem;
}

.event-modal {
  width: min(1180px, 100%);
  max-height: 92vh;
  overflow: auto;
  background: #fffdf9;
  border-radius: 28px;
  border: 2px solid #e8dfd0;
  box-shadow: 0 30px 90px rgba(26, 17, 8, 0.22);
  position: relative;
}

.event-modal__close {
  position: absolute;
  top: 14px;
  right: 14px;
  width: 42px;
  height: 42px;
  border-radius: 50%;
  border: none;
  background: #1a1108;
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 3;
}

.event-modal__header {
  --accent: #ff4d1c;
  background: var(--accent);
  padding: 2rem 2rem 1.6rem;
  color: #fff;
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 16px;
  border-radius: 26px 26px 0 0;
  flex-wrap: wrap;
}

.event-modal__day {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.event-modal__day-name {
  font-family: 'Playfair Display', serif;
  font-size: clamp(2rem, 4vw, 3.2rem);
  line-height: 0.95;
}

.event-modal__day-date {
  font-size: 0.82rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 0.14em;
  opacity: 0.82;
}

.event-modal__status {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: 0.75rem;
  font-weight: 800;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 10px 16px;
  border-radius: 100px;
  background: rgba(255, 255, 255, 0.16);
  border: 1.5px solid rgba(255, 255, 255, 0.28);
  color: #fff;
}

.event-modal__content {
  padding: 2rem;
  display: grid;
  grid-template-columns: 1.5fr 0.9fr;
  gap: 24px;
}

.event-modal__time {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  font-size: 0.78rem;
  font-weight: 800;
  color: #7a6e62;
  background: #f4efe8;
  padding: 7px 12px;
  border-radius: 100px;
  margin-bottom: 1rem;
}

.event-modal__title {
  font-family: 'Playfair Display', serif;
  font-size: clamp(2rem, 4vw, 3.4rem);
  line-height: 0.95;
  color: #1a1108;
  margin-bottom: 1rem;
}

.event-modal__desc {
  font-size: 1rem;
  color: #6d6258;
  line-height: 1.8;
  margin-bottom: 1.6rem;
}

.event-info-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 16px;
}

.event-info-card {
  border: 1.5px solid #e8dfd0;
  background: #fff;
  border-radius: 18px;
  padding: 1rem;
}

.event-info-card__label {
  display: block;
  font-size: 0.7rem;
  font-weight: 800;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #a09488;
  margin-bottom: 0.65rem;
}

.event-info-card__value {
  display: flex;
  align-items: flex-start;
  gap: 8px;
  font-size: 0.92rem;
  color: #433a32;
  line-height: 1.5;
  font-weight: 600;
}

.event-info-card__value--link a {
  color: #ff4d1c;
  font-weight: 800;
  word-break: break-word;
  text-decoration: none;
}

.event-aside-card {
  background: #fff;
  border: 1.5px solid #e8dfd0;
  border-radius: 22px;
  padding: 1.2rem;
  position: sticky;
  top: 0;
}

.event-aside-card__eyebrow {
  display: inline-block;
  font-size: 0.7rem;
  font-weight: 800;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: #a09488;
  margin-bottom: 0.8rem;
}

.event-aside-card__title {
  font-family: 'Playfair Display', serif;
  font-size: 1.55rem;
  line-height: 1.1;
  margin-bottom: 0.8rem;
  color: #1a1108;
}

.event-aside-card__text {
  font-size: 0.92rem;
  line-height: 1.7;
  color: #6d6258;
  margin-bottom: 1rem;
}

.event-aside-card__button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: #ff4d1c;
  color: #fff;
  padding: 12px 16px;
  border-radius: 100px;
  font-size: 0.82rem;
  font-weight: 800;
  letter-spacing: 0.04em;
  text-decoration: none;
}

.event-aside-card__empty {
  font-size: 0.85rem;
  color: #a09488;
  font-weight: 700;
}

/* FOOTER */
.su-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 18px;
  flex-wrap: wrap;
  padding: 2.2rem 2.5rem;
  background: linear-gradient(90deg, #140a04 0%, #241108 100%);
  border-top: 1px solid rgba(255, 255, 255, 0.06);
}

.su-footer__brand {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.footer-mark {
  font-family: 'Playfair Display', serif;
  font-size: 2.2rem;
  font-weight: 900;
  color: #fdf8f0;
  letter-spacing: -0.02em;
}

.footer-mark em {
  font-style: normal;
  color: var(--r);
}

.su-footer__tag {
  font-size: 0.82rem;
  font-weight: 500;
  color: rgba(253, 248, 240, 0.62);
  line-height: 1.6;
  text-transform: uppercase;
  letter-spacing: 0.06em;
}

.su-footer__credits {
  display: flex;
  align-items: center;
  gap: 0.55rem;
  font-size: 0.92rem;
  color: rgba(253, 248, 240, 0.72);
}

.su-footer__credits a {
  color: #fff;
  font-weight: 800;
  text-decoration: none;
  border-bottom: 1px solid rgba(255, 255, 255, 0.3);
}

.su-footer__credits a:hover {
  color: #ffb39d;
  border-bottom-color: #ffb39d;
}

/* RESPONSIVE */
@media (max-width: 1024px) {
  .hero__content {
    padding: 5rem 1.5rem 2rem;
  }

  .hero__bg-letter {
    right: -10vw;
    font-size: clamp(260px, 46vw, 520px);
  }

  .stat {
    padding: 0 1.5rem;
  }

  .event-modal__content {
    grid-template-columns: 1fr;
  }

  .event-aside-card {
    position: static;
  }
}

@media (max-width: 768px) {
  .hero {
    min-height: auto;
  }

  .hero__content {
    padding: 4.5rem 1.25rem 1.75rem;
  }

  .hero__eyebrow {
    font-size: 0.78rem;
    gap: 12px;
    letter-spacing: 0.16em;
  }

  .heading__semana {
    font-size: clamp(3rem, 14vw, 5.2rem);
  }

  .heading__u {
    font-size: clamp(3.8rem, 16vw, 6rem);
    margin-left: 0.04em;
    margin-right: 0.02em;
  }

  .heading__year {
    font-size: clamp(1.8rem, 6vw, 3rem);
  }

  .hero__bg-letter {
    font-size: clamp(220px, 44vw, 380px);
    right: -16vw;
    top: 42%;
  }

  .shape--circle-lg {
    width: 180px;
    height: 180px;
    top: -60px;
    right: -60px;
  }

  .shape--pill,
  .shape--ring {
    display: none;
  }

  .access-legend__inner {
    grid-template-columns: 1fr;
    padding: 0 1rem;
  }

  .agenda {
    padding: 0 1rem 4rem;
  }

  .day-section {
    margin-top: 2.2rem;
  }

  .day-band {
    padding: 1rem;
    border-radius: 18px;
  }

  .day-band__index {
    display: none;
  }

  .day-band__name {
    font-size: 2rem;
  }

  .cards-grid {
    grid-template-columns: 1fr;
    gap: 14px;
  }

  .ev-card {
    min-height: 310px;
    padding: 18px 16px 16px;
  }

  .ev-card__name {
    font-size: 1.08rem;
  }

  .ev-card__desc {
    font-size: 0.78rem;
    min-height: auto;
  }

  .ev-card__meta {
    min-height: auto;
  }

  .ev-card__footer {
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
  }

  .event-modal-overlay {
    align-items: center;
    padding: 0.75rem;
  }

  .event-modal__header {
    padding: 1.5rem 1.2rem 1.2rem;
  }

  .event-modal__content {
    padding: 1.2rem;
  }

  .event-info-grid {
    grid-template-columns: 1fr;
  }

  .organizers-band {
    padding: 2rem 0 4rem;
  }

  .organizers-band__header {
    padding: 0 1rem;
  }

  .organizers-band__header h2 {
    font-size: clamp(2rem, 8vw, 3rem);
  }

  .organizers-band__header p {
    font-size: 0.95rem;
  }

  .organizers-marquee__fade {
    width: 42px;
  }

  .organizer-card {
    width: 180px;
    min-width: 180px;
    max-width: 180px;
    min-height: 220px;
    padding: 1rem 0.8rem;
    border-radius: 20px;
  }

  .organizer-card__logo {
    width: 88px;
    height: 88px;
    border-radius: 18px;
  }

  .organizer-card__name {
    font-size: 0.8rem;
    min-height: 3em;
  }

  .organizer-card__ig {
    font-size: 0.74rem;
  }

  .organizers-marquee__track {
    gap: 14px;
    padding: 0 1rem;
    animation-duration: 28s;
  }
}

@media (max-width: 640px) {
  .hero__content {
    padding: 4rem 1rem 1.5rem;
  }

  .hero__eyebrow {
    gap: 10px;
    font-size: 0.72rem;
    letter-spacing: 0.14em;
    margin-bottom: 1.2rem;
  }

  .eyebrow__line {
    max-width: 30px;
  }

  .heading__semana {
    font-size: clamp(2.7rem, 16vw, 4.2rem);
  }

  .heading__u {
    font-size: clamp(3.5rem, 18vw, 5rem);
  }

  .heading__year {
    font-size: clamp(1.5rem, 7vw, 2.2rem);
    padding-bottom: 0.15em;
  }

  .hero__bg-letter {
    font-size: 260px;
    right: -28vw;
    top: 36%;
    -webkit-text-stroke: 1.5px rgba(255, 77, 28, 0.13);
  }

  .shape--circle-sm,
  .shape--square {
    display: none;
  }

  .hero__desc {
    font-size: 0.95rem;
    line-height: 1.65;
    margin-bottom: 1.8rem;
  }

  .hero__scroll {
    font-size: 0.72rem;
  }

  .hero__stats {
    margin-top: 2rem;
    padding: 1rem;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
  }

  .stat {
    flex: 1 1 calc(50% - 10px);
    min-width: 140px;
    max-width: 220px;
    background: rgba(255, 255, 255, 0.04);
    border-radius: 14px;
    padding: 1rem 0.75rem;
  }

  .stat__n {
    font-size: 2rem;
  }

  .stat__l {
    font-size: 0.62rem;
    text-align: center;
  }

  .stat__div {
    display: none;
  }

  .cards-grid {
    grid-template-columns: 1fr;
    gap: 14px;
  }

  .ev-card {
    padding: 18px 16px 14px;
    min-height: auto;
  }

  .ev-card__name {
    font-size: 1.15rem;
  }

  .ev-card__footer {
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
  }

  .access-legend__item {
    padding: 0.9rem;
  }

  .access-legend__label {
    font-size: 0.88rem;
  }

  .access-legend__sub {
    font-size: 0.76rem;
  }

  .organizers-band__header {
    padding: 0 1rem;
  }

  .organizers-band__header h2 {
    font-size: clamp(2rem, 10vw, 3rem);
  }

  .organizers-band__header p {
    font-size: 0.92rem;
  }

  .organizers-marquee__fade {
    width: 36px;
  }

  .organizer-card {
    width: 160px;
    min-width: 160px;
    max-width: 160px;
    min-height: 205px;
    padding: 0.9rem 0.65rem;
    border-radius: 18px;
  }

  .organizer-card__logo {
    width: 78px;
    height: 78px;
    border-radius: 16px;
    padding: 8px;
  }

  .organizer-card__name {
    font-size: 0.73rem;
    min-height: 3.2em;
  }

  .organizer-card__ig {
    font-size: 0.69rem;
  }

  .organizers-marquee__track {
    gap: 12px;
    padding: 0 1rem;
    animation-duration: 24s;
  }

  .event-modal__close {
    top: 10px;
    right: 10px;
    width: 38px;
    height: 38px;
  }

  .event-modal__day-name {
    font-size: 1.8rem;
  }

  .event-modal__title {
    font-size: 2rem;
  }

  .event-aside-card__title {
    font-size: 1.3rem;
  }

  .su-footer {
    flex-direction: column;
    align-items: flex-start;
    padding: 1.8rem 1.25rem;
  }

  .su-footer__tag {
    font-size: 0.75rem;
  }
}
</style>