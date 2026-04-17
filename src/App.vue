<template>
  <div class="su-app">
    <!-- ══ HERO ══════════════════════════════════════ -->
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

    <!-- ══ LEYENDA ═════════════════════════════════ -->
    <section class="legend-section">
      <div class="legend-section__inner">
        <div class="legend-card legend-card--open">
          <div class="legend-card__icon">
            <Globe :size="18" />
          </div>
          <div>
            <h3>Abierta al público</h3>
            <p>Disponible para toda la comunidad universitaria y público general.</p>
          </div>
        </div>

        <div class="legend-card legend-card--limited">
          <div class="legend-card__icon">
            <Lock :size="18" />
          </div>
          <div>
            <h3>Solo para la carrera</h3>
            <p>Actividad dirigida a estudiantes de una carrera específica.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- ══ FILTROS ═════════════════════════════════ -->
    <section id="agenda" class="agenda-controls-wrap">
      <div class="agenda-toolbar">
        <div class="agenda-toolbar__head">
          <span class="agenda-toolbar__eyebrow">Explora la programación</span>
          <h2>Programa de actividades</h2>
          <p>Busca por nombre, lugar, asociación o filtra por día y tipo de acceso.</p>
        </div>

        <div class="agenda-controls">
          <div class="control control--search">
            <Search :size="18" />
            <input
              v-model="searchQuery"
              type="text"
              placeholder="Buscar actividad, lugar o asociación"
            />
          </div>

          <div class="control">
            <Filter :size="18" />
            <select v-model="selectedDay">
              <option value="all">Todos los días</option>
              <option
                v-for="dia in agenda"
                :key="dia.nombre"
                :value="dia.nombre"
              >
                {{ dia.nombre }}
              </option>
            </select>
          </div>

          <div class="control">
            <Layers3 :size="18" />
            <select v-model="selectedAccess">
              <option value="all">Todo acceso</option>
              <option value="open">Abiertas</option>
              <option value="limited">Limitadas</option>
            </select>
          </div>
        </div>
      </div>
    </section>

    <!-- ══ AGENDA ═══════════════════════════════════ -->
    <main class="agenda">
      <section
        v-for="(dia, di) in filteredAgenda"
        :key="dia.nombre"
        class="day-section"
      >
        <div class="day-band" :style="{ '--band': DAY_COLORS[getDayIndex(dia.nombre)] }">
          <div class="day-band__left">
            <span class="day-band__index">{{ String(getDayIndex(dia.nombre) + 1).padStart(2, '0') }}</span>
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
            :class="['ev-card', `ev-card--skin${(getDayIndex(dia.nombre) * 7 + ei) % 5}`]"
            role="button"
            tabindex="0"
            @click="openEvent(ev, dia)"
            @keydown.enter="openEvent(ev, dia)"
            @keydown.space.prevent="openEvent(ev, dia)"
          >
            <div
              class="ev-card__dot"
              :style="{ background: DAY_COLORS[getDayIndex(dia.nombre)] }"
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
              :style="{ background: DAY_COLORS[getDayIndex(dia.nombre)] }"
            ></div>
          </article>
        </div>
      </section>

      <div v-if="!filteredAgenda.length" class="empty-state">
        <SearchX :size="28" />
        <h3>No encontramos actividades</h3>
        <p>Prueba con otro término de búsqueda o cambia los filtros.</p>
      </div>
    </main>

    <!-- ══ ASOCIACIONES ════════════════════════════ -->
    <section class="organizers-band">
      <div class="organizers-band__inner">
        <div class="organizers-band__header">
          <span class="organizers-band__eyebrow">Participación estudiantil</span>
          <h2>Asociaciones que organizan</h2>
          <p>
            Agrupaciones estudiantiles que hacen posible la Semana U 2026 en la Sede Guanacaste.
          </p>
        </div>

        <div class="organizers-band__slider">
          <div class="organizers__fade organizers__fade--left"></div>
          <div class="organizers__fade organizers__fade--right"></div>

          <div class="organizers__track">
            <a
              v-for="(org, index) in organizadoresDuplicados"
              :key="index"
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

    <!-- ══ MODAL ═══════════════════════════════════ -->
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

    <!-- ══ FOOTER ══════════════════════════════════ -->
    <footer class="su-footer">
      <div class="su-footer__brand">
        <span class="footer-mark">SU<em>'26</em></span>
        <p class="su-footer__tag">
          Universidad de Costa Rica · Sede Guanacaste · 20–25 de abril 2026
        </p>
      </div>

      <div class="su-footer__credits">
        <span>Hecho por</span>
        <a
          href="https://www.instagram.com/oscar_herraa13/"
          target="_blank"
          rel="noopener noreferrer"
        >
          Oscar Herra
        </a>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { computed, ref, watch, onBeforeUnmount } from 'vue';
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
  Search,
  Filter,
  Layers3,
  SearchX
} from 'lucide-vue-next';

import logoAedg from '@/assets/aedg.jpeg';
import logoAeep from '@/assets/aeep.jpeg';
import logoAeg from '@/assets/aeg.JPG';
import logoAete from '@/assets/aete.png';
import logoAgia from '@/assets/agia.jpeg';
import logoAsoadu from '@/assets/asoadu.jpeg';
import logoDn from '@/assets/dn.png';

/* Si aún no tienes estos archivos, puedes dejarlos como placeholder o importarlos luego */
// import logoAei from '@/assets/aei.png';
// import logoAsoinfo from '@/assets/asoinfo.png';
// import logoAsam from '@/assets/asam.png';

const DAY_COLORS = ['#FF4D1C', '#00B896', '#F5B800', '#D94FD5', '#2563EB', '#16A34A'];

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
};

const organizadores = [
  {
    nombre: 'Asoc. Estudiantes Sede Guanacaste',
    handle: '@ucr.aeg',
    instagram: instagramLinks.aeg,
    logo: logoAeg
  },
  {
    nombre: 'Asoc. Estudiantes de Turismo',
    handle: '@aete.sg',
    instagram: instagramLinks.aete,
    logo: logoAete
  },
  {
    nombre: 'Asoc. Estudiantes Informática',
    handle: '@asoinfo_sg',
    instagram: instagramLinks.asoinfo,
    logo: 'https://via.placeholder.com/90x90/FDF8F0/1A1108?text=INFO'
  },
  {
    nombre: 'Asoc. Estudiantes Educación Primaria',
    handle: '@aecep_ucr',
    instagram: instagramLinks.acep,
    logo: logoAeep
  },
  {
    nombre: 'Asoc. Estudiantes de Aduanas',
    handle: '@asoadu_sg',
    instagram: instagramLinks.asoadu,
    logo: logoAsoadu
  },
  {
    nombre: 'Asoc. Estudiantes de Inglés',
    handle: '@aei_ucr_sg',
    instagram: instagramLinks.aei,
    logo: 'https://via.placeholder.com/90x90/FDF8F0/1A1108?text=AEI'
  },
  {
    nombre: 'Asoc. Estudiantes Salud Ambiental',
    handle: '@asam.ucrsg',
    instagram: instagramLinks.asam,
    logo: 'https://via.placeholder.com/90x90/FDF8F0/1A1108?text=ASAM'
  },
  {
    nombre: 'Asoc. Estudiantes de Derecho',
    handle: '@aso_derecho_gte',
    instagram: instagramLinks.aedg,
    logo: logoAedg
  },
  {
    nombre: 'Asoc. Estudiantes Dirección de Negocios',
    handle: '@dn_guanacaste',
    instagram: instagramLinks.dn,
    logo: logoDn
  },
  {
    nombre: 'Asoc. Estudiantes Ing. Alimentos',
    handle: '@a.g.i.a',
    instagram: instagramLinks.agia,
    logo: logoAgia
  },
];

const organizadoresDuplicados = computed(() => [...organizadores, ...organizadores]);

const agenda = [
  {
    nombre: 'Lunes',
    fecha: '20 de Abril',
    actividades: [
      { hora: '08:00 a.m. - 12:00 m.d.', nombre: 'Voleibol en la plaza', lugar: 'Plaza', descripcion: 'Actividad recreativa de voleibol para estudiantes.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '09:00 a.m.', nombre: 'Quéjese', lugar: 'Pasillos 1, 2 y 3 pabellón', descripcion: 'Espacio abierto para expresar inquietudes sobre la Sede y proponer mejoras.', responsable: 'Asoc. Estudiantes de Inglés', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '10:00 a.m.', nombre: 'Picnic de "Traje"', lugar: 'Área verde contiguo al Ranchito', descripcion: 'Picnic temático con las personas estudiantes.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '10:00 a.m.', nombre: 'Rally DN', lugar: 'Cancha de Futbol', descripcion: 'Rally temático con pruebas y retos para estudiantes.', responsable: 'Asoc. Estudiantes Dirección de Negocios', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '12:00 m.d.', nombre: 'Gran Bingo', lugar: 'Salón Multiusos', descripcion: 'Bingo masivo con capacidad para 150 personas y muchos premios.', responsable: 'Asoc. Estudiantes Ing. Alimentos', publico: 'Abierta a Público General' },
      { hora: '01:00 p.m.', nombre: 'Just Dance', lugar: 'Biblioteca', descripcion: 'Concurso de baile con el videojuego Just Dance.', responsable: 'Asoc. Estudiantes de Inglés', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '01:00 p.m.', nombre: 'Búsqueda del Tesoro', lugar: 'Zonas verdes', descripcion: 'Actividad de búsqueda de objetos escondidos en el campus.', responsable: 'Asoc. Estudiantes Educación Primaria', publico: 'Limitada a la población de la carrera' },
      { hora: '02:00 p.m.', nombre: 'Olimpiadas de Debate', lugar: 'Sala de Juicios', descripcion: 'Concurso de argumentación y oratoria para estudiantes de Derecho.', responsable: 'Asoc. Estudiantes de Derecho', publico: 'Limitada a la población de la carrera' },
    ]
  },
  {
    nombre: 'Martes',
    fecha: '21 de Abril',
    actividades: [
      { hora: '09:00 a.m.', nombre: 'Spelling Bee', lugar: 'Miniauditorio', descripcion: 'Concurso de deletreo en inglés con múltiples rondas.', responsable: 'Asoc. Estudiantes de Inglés', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '10:00 a.m.', nombre: 'Taller de Totebags', lugar: 'Salón Multiusos', descripcion: 'Taller de pintura y personalización de bolsas de tela.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '10:00 a.m.', nombre: 'Corrige el Código', lugar: 'Espacio Virtual', descripcion: 'Reto de análisis y depuración de código para Informática.', responsable: 'Asoc. Estudiantes Informática', publico: 'Limitada a la Población de la Carrera' },
      { hora: '01:00 p.m.', nombre: 'Karaoke', lugar: 'Aula 20', descripcion: 'Tarde de karaoke para demostrar el talento vocal de los estudiantes.', responsable: 'Asoc. Estudiantes de Inglés', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '01:00 p.m. - 05:00 p.m.', nombre: 'Día de shows de talentos', lugar: 'Salón Multiusos', descripcion: 'Presentaciones de talento estudiantil en distintas disciplinas.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
    ]
  },
  {
    nombre: 'Miércoles',
    fecha: '22 de Abril',
    actividades: [
      { hora: '08:00 a.m.', nombre: 'Cine Bloque A', lugar: 'Aula 3', descripcion: 'Sesión de cine matutina para empezar el día con buena energía.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '08:00 a.m. - 10:30 a.m.', nombre: 'Recorriendo el sendero de la sede', lugar: 'Sendero de la sede', descripcion: 'Recorridos en el sendero con actividades de sensibilización y aprovechamiento de recursos naturales, guiado por estudiantes avanzados de turismo.', responsable: 'Asoc. Estudiantes de Turismo', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '09:00 a.m. - 12:00 m.d.', nombre: 'Juegos recreativos', lugar: 'A la par del edificio', descripcion: 'Juegos de mesa, ping pong, bádminton y más.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '10:00 a.m.', nombre: 'Picnic', lugar: 'Nuevas mesitas', descripcion: 'Actividad para compartir al aire libre con compañeros.', responsable: 'Asoc. Estudiantes de Inglés', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '12:00 m.d.', nombre: 'Cine Bloque B', lugar: 'Aula 16', descripcion: 'Sesión de cine al mediodía.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '01:00 p.m. - 04:00 p.m.', nombre: 'Karaoke', lugar: 'Salón Multiusos', descripcion: 'Actividad recreativa de canto para estudiantes.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '02:00 p.m.', nombre: 'Tarde de Cine', lugar: 'Aula a definir', descripcion: 'Tarde de convivio y películas seleccionadas por la carrera.', responsable: 'Asoc. Estudiantes Salud Ambiental', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '03:00 p.m.', nombre: 'Tardeada de Artes', lugar: 'Salón Multiusos', descripcion: 'Espacio creativo de artes plásticas y expresión artística.', responsable: 'Asoc. Estudiantes de Turismo', publico: 'Limitada a la Población de la Carrera' },
      { hora: '04:00 p.m.', nombre: 'Cine Bloque C', lugar: 'Aula 19', descripcion: 'Cierre vespertino del miércoles de cine.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
    ]
  },
  {
    nombre: 'Jueves',
    fecha: '23 de Abril',
    actividades: [
      { hora: '08:00 a.m. - 12:00 m.d.', nombre: 'Fútbol sala', lugar: 'Gimnasio', descripcion: 'Actividad deportiva recreativa de fútbol sala.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '09:00 a.m.', nombre: 'Búsqueda del Tesoro', lugar: 'Diferentes áreas', descripcion: 'Búsqueda de pistas por todo el campus para Turismo.', responsable: 'Asoc. Estudiantes de Turismo', publico: 'Limitada a la Población de la Carrera' },
      { hora: '10:00 a.m.', nombre: 'Torneo Play Station', lugar: 'Biblioteca', descripcion: 'Torneo de videojuegos en consola abierto a toda la comunidad.', responsable: 'Asoc. Estudiantes de Inglés', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '01:00 p.m.', nombre: 'Convivio', lugar: 'Rancho de Deportivas', descripcion: 'Convivio exclusivo para la carrera de Salud Ambiental.', responsable: 'Asoc. Estudiantes Salud Ambiental', publico: 'Limitada a la Población de la Carrera' },
      { hora: '01:00 p.m. - 03:00 p.m.', nombre: 'Tardeada de arte', lugar: 'Salón Multiusos (por confirmar)', descripcion: 'Concurso de pintura sobre lienzo con reconocimiento para las 3 mejores obras. Pendiente por disponibilidad de multiusos y confirmación de profesora de arte.', responsable: 'Asoc. Estudiantes de Turismo', publico: 'Limitada a la Población de la Carrera' },
      { hora: '02:00 p.m. - 04:00 p.m.', nombre: 'Charlas con expositores invitados', lugar: 'Auditorio', descripcion: 'Expositores invitados expondrán temas de interés relacionados con el turismo y los recursos naturales o turísticos. Temas específicos por confirmar.', responsable: 'Asoc. Estudiantes de Turismo', publico: 'Limitada a la Población de la Carrera' },
      { hora: '02:00 p.m.', nombre: 'Karaoke Clásico', lugar: 'Salón Multiusos', descripcion: 'Tarde de karaoke con los éxitos de todos los tiempos.', responsable: 'Asoc. Estudiantes Sede Guanacaste', publico: 'Abierta a la Comunidad Universitaria' },
      { hora: '02:00 p.m.', nombre: '¿Quién quiere ser juriste?', lugar: 'Miniauditorio', descripcion: 'Concurso de conocimientos jurídicos al estilo de programa de TV.', responsable: 'Asoc. Estudiantes de Derecho', publico: 'Limitada a la Población de la Carrera' },
    ]
  },
  {
    nombre: 'Viernes',
    fecha: '24 de Abril',
    actividades: [
      { hora: '09:00 a.m. - 04:00 p.m.', nombre: 'Búsqueda del tesoro', lugar: 'Distintos sitios de la sede', descripcion: 'Se brindarán pistas sobre objetos escondidos alrededor de la Sede y quien los encuentre será la persona ganadora de dicho objeto.', responsable: 'Asoc. Estudiantes de Turismo', publico: 'Limitada a la Población de la Carrera' },
      { hora: '09:00 a.m. - 12:00 p.m.', nombre: 'Rally de obstáculos', lugar: 'Cancha de futbol de la sede', descripcion: 'Desarrollo de actividades en parejas o equipos con obstáculos y estaciones.', responsable: 'Asoc. Estudiantes de Turismo', publico: 'Limitada a la Población de la Carrera' },
      { hora: '02:00 p.m.', nombre: 'Charla con Expertos', lugar: 'Miniauditorio', descripcion: 'Charlas orientadas a reforzar áreas clave de la carrera.', responsable: 'Asoc. Estudiantes de Turismo', publico: 'Limitada a la Población de la Carrera' },
      { hora: 'Todo el día', nombre: 'Olimpiadas Ambientales', lugar: 'Instalaciones deportivas', descripcion: 'Jornada completa de actividades recreativas para Salud Ambiental.', responsable: 'Asoc. Estudiantes Salud Ambiental', publico: 'Limitada a la Población de la Carrera' },
    ]
  },
  {
    nombre: 'Sábado',
    fecha: '25 de Abril',
    actividades: [
      { hora: '01:00 p.m.', nombre: 'Karaoke de Cierre', lugar: 'Miniauditorio', descripcion: 'Tarde de karaoke para despedir la Semana U 2026 con todo.', responsable: 'Asoc. Estudiantes de Aduanas', publico: 'Limitada a Aduanas' },
    ]
  }
];

const selectedEvent = ref(null);
const selectedEventDay = ref(null);
const selectedEventColor = ref(DAY_COLORS[0]);

const searchQuery = ref('');
const selectedDay = ref('all');
const selectedAccess = ref('all');

function normalizeText(text = '') {
  return text
    .toLowerCase()
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '');
}

function isOpen(ev) {
  return normalizeText(ev.publico).includes('abierta');
}

function getDayIndex(dayName) {
  return agenda.findIndex((d) => d.nombre === dayName);
}

function getInstagram(ev) {
  const responsable = normalizeText(ev?.responsable || '');

  if (responsable.includes('sede guanacaste')) return instagramLinks.aeg;
  if (responsable.includes('turismo')) return instagramLinks.aete;
  if (responsable.includes('informatica')) return instagramLinks.asoinfo;
  if (responsable.includes('educacion primaria')) return instagramLinks.acep;
  if (responsable.includes('aduanas')) return instagramLinks.asoadu;
  if (responsable.includes('ingles')) return instagramLinks.aei;
  if (responsable.includes('salud ambiental')) return instagramLinks.asam;
  if (responsable.includes('derecho')) return instagramLinks.aedg;
  if (responsable.includes('direccion de negocios')) return instagramLinks.dn;
  if (responsable.includes('direccion de empresas')) return instagramLinks.dn;
  if (responsable.includes('ing. alimentos')) return instagramLinks.agia;
  if (responsable.includes('ingenieria de alimentos')) return instagramLinks.agia;

  return '';
}

function getInstagramHandle(ev) {
  const url = getInstagram(ev);
  if (!url) return 'No disponible';
  const clean = url.replace('https://www.instagram.com/', '').replaceAll('/', '');
  return `@${clean}`;
}

function matchesSearch(ev) {
  const q = normalizeText(searchQuery.value.trim());
  if (!q) return true;

  const haystack = normalizeText(
    `${ev.nombre} ${ev.descripcion} ${ev.lugar} ${ev.responsable} ${ev.publico} ${ev.hora}`
  );

  return haystack.includes(q);
}

function matchesAccess(ev) {
  if (selectedAccess.value === 'all') return true;
  if (selectedAccess.value === 'open') return isOpen(ev);
  if (selectedAccess.value === 'limited') return !isOpen(ev);
  return true;
}

const filteredAgenda = computed(() => {
  return agenda
    .filter((dia) => selectedDay.value === 'all' || dia.nombre === selectedDay.value)
    .map((dia) => ({
      ...dia,
      actividades: dia.actividades.filter((ev) => matchesSearch(ev) && matchesAccess(ev)),
    }))
    .filter((dia) => dia.actividades.length > 0);
});

function openEvent(ev, dayObj) {
  selectedEvent.value = ev;
  selectedEventDay.value = dayObj;
  selectedEventColor.value = DAY_COLORS[getDayIndex(dayObj.nombre)];
}

function closeEvent() {
  selectedEvent.value = null;
  selectedEventDay.value = null;
}

function handleKeydown(e) {
  if (e.key === 'Escape' && selectedEvent.value) {
    closeEvent();
  }
}

watch(selectedEvent, (value) => {
  document.body.style.overflow = value ? 'hidden' : '';
});

window.addEventListener('keydown', handleKeydown);

onBeforeUnmount(() => {
  document.body.style.overflow = '';
  window.removeEventListener('keydown', handleKeydown);
});

const totalActividades = computed(() =>
  agenda.reduce((s, d) => s + d.actividades.length, 0)
);

const totalAbiertas = computed(() =>
  agenda.reduce((s, d) => s + d.actividades.filter(isOpen).length, 0)
);

const totalLimitadas = computed(() =>
  totalActividades.value - totalAbiertas.value
);
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@900&family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap');

:root {
  --cream: #FDF8F0;
  --ink: #1A1108;
  --muted: #7A6E62;
  --border: #E8DFD0;
  --r: #FF4D1C;
  --g: #00B896;
  --y: #F5B800;
  --p: #D94FD5;
  --b: #2563EB;
  --gr: #16A34A;
}

.su-app {
  background: #FDF8F0;
  color: #1A1108;
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
  background: #FDF8F0;
  border-bottom: 2.5px solid #1A1108;
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
  opacity: .9;
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
  opacity: .85;
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
  gap: 12px;
  font-size: .72rem;
  font-weight: 600;
  letter-spacing: .18em;
  text-transform: uppercase;
  color: #7A6E62;
  margin-bottom: 1.8rem;
}

.eyebrow__line {
  flex: 1;
  max-width: 50px;
  height: 1.5px;
  background: #7A6E62;
}

.hero__heading {
  display: flex;
  align-items: baseline;
  gap: .06em;
  flex-wrap: wrap;
  margin-bottom: 2rem;
  line-height: .9;
}

.heading__semana {
  font-family: 'Playfair Display', serif;
  font-weight: 900;
  font-size: clamp(3.5rem, 11vw, 9rem);
  color: #1A1108;
  letter-spacing: -.02em;
}

.heading__u {
  font-family: 'Playfair Display', serif;
  font-weight: 900;
  font-size: clamp(4.2rem, 12.5vw, 10.2rem);
  color: var(--r);
  letter-spacing: -.015em;
  margin-left: .08em;
  margin-right: .04em;
  line-height: .88;
}

.heading__year {
  font-family: 'Playfair Display', serif;
  font-weight: 900;
  font-size: clamp(2rem, 5vw, 4rem);
  color: #1A1108;
  letter-spacing: -.01em;
  align-self: flex-end;
  padding-bottom: .1em;
  opacity: .35;
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
  font-size: .78rem;
  font-weight: 700;
  padding: 8px 16px;
  border-radius: 100px;
  border: 2px solid transparent;
}

.meta-chip--dark { background: #1A1108; color: #FDF8F0; }
.meta-chip--orange { background: var(--r); color: #fff; }
.meta-chip--teal { background: var(--g); color: #fff; }

.hero__desc {
  max-width: 520px;
  font-size: 1rem;
  font-weight: 300;
  line-height: 1.7;
  color: #7A6E62;
  margin-bottom: 2.4rem;
}

.hero__scroll {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: .78rem;
  font-weight: 700;
  letter-spacing: .1em;
  text-transform: uppercase;
  color: #7A6E62;
  text-decoration: none;
  animation: bounce 2.4s ease-in-out infinite;
}

@keyframes bounce {
  0%,100% { transform: translateY(0); }
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
  background: #1A1108;
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
  color: #FDF8F0;
  line-height: 1;
}

.stat__l {
  font-size: .65rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .12em;
  color: rgba(253,248,240,.4);
}

.stat__div {
  width: 1px;
  height: 40px;
  background: rgba(253,248,240,.15);
}

/* LEYENDA */
.legend-section {
  max-width: 1280px;
  margin: 0 auto;
  padding: 1.5rem 1.5rem 0;
}

.legend-section__inner {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 14px;
}

.legend-card {
  display: flex;
  align-items: flex-start;
  gap: 14px;
  padding: 1.2rem 1.3rem;
  border-radius: 22px;
  border: 2px solid #E8DFD0;
  background: #fff;
}

.legend-card__icon {
  width: 42px;
  height: 42px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.legend-card--open .legend-card__icon {
  background: #E6FBF5;
  color: #00875F;
}

.legend-card--limited .legend-card__icon {
  background: #FDF0FD;
  color: #9C2D9C;
}

.legend-card h3 {
  font-size: 1rem;
  font-weight: 800;
  margin-bottom: .2rem;
  color: #1A1108;
}

.legend-card p {
  font-size: .85rem;
  color: #7A6E62;
  line-height: 1.55;
}

/* FILTROS */
.agenda-controls-wrap {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem 1.5rem 0;
}

.agenda-toolbar {
  background: #fff;
  border: 2px solid #E8DFD0;
  border-radius: 28px;
  padding: 1.35rem;
  box-shadow: 0 12px 30px rgba(26,17,8,.04);
}

.agenda-toolbar__head {
  margin-bottom: 1rem;
}

.agenda-toolbar__eyebrow {
  display: inline-block;
  font-size: .72rem;
  font-weight: 800;
  letter-spacing: .14em;
  text-transform: uppercase;
  color: #7A6E62;
  margin-bottom: .5rem;
}

.agenda-toolbar__head h2 {
  font-family: 'Playfair Display', serif;
  font-size: clamp(1.8rem, 3vw, 2.6rem);
  line-height: 1;
  margin-bottom: .45rem;
}

.agenda-toolbar__head p {
  color: #7A6E62;
  font-size: .95rem;
}

.agenda-controls {
  display: grid;
  grid-template-columns: 1.7fr .8fr .8fr;
  gap: 12px;
}

.control {
  display: flex;
  align-items: center;
  gap: 10px;
  background: #FDF8F0;
  border: 2px solid #E8DFD0;
  border-radius: 18px;
  padding: 0 14px;
  min-height: 56px;
}

.control svg {
  color: #7A6E62;
  flex-shrink: 0;
}

.control input,
.control select {
  width: 100%;
  border: none;
  outline: none;
  background: transparent;
  font-family: inherit;
  font-size: .95rem;
  color: #433a32;
}

.control select {
  cursor: pointer;
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
  background: var(--band, #FF4D1C);
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
  color: rgba(255,255,255,.25);
  line-height: 1;
}

.day-band__name {
  font-family: 'Playfair Display', serif;
  font-size: clamp(1.8rem, 5vw, 3rem);
  font-weight: 900;
  color: #fff;
  line-height: 1;
  letter-spacing: -.01em;
}

.day-band__date {
  font-size: .78rem;
  font-weight: 600;
  color: rgba(255,255,255,.78);
  text-transform: uppercase;
  letter-spacing: .1em;
  margin-top: 3px;
}

.day-band__pill {
  background: rgba(255,255,255,.2);
  color: #fff;
  font-size: .72rem;
  font-weight: 800;
  letter-spacing: .06em;
  text-transform: uppercase;
  padding: 6px 16px;
  border-radius: 100px;
  border: 1.5px solid rgba(255,255,255,.35);
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(270px, 1fr));
  gap: 16px;
}

.ev-card {
  position: relative;
  background: #fff;
  border: 2.5px solid #E8DFD0;
  border-radius: 20px;
  padding: 22px 20px 18px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  transition: transform .2s ease, box-shadow .2s ease, border-color .2s ease;
  cursor: pointer;
}

.ev-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 20px 50px rgba(26,17,8,.10);
  border-color: #1A1108;
}

.ev-card--skin0 { background: #ffffff; }
.ev-card--skin1 { background: #FFFBF0; }
.ev-card--skin2 { background: #F0FDF8; }
.ev-card--skin3 { background: #FFF5FD; }
.ev-card--skin4 { background: #F0F5FF; }

.ev-card__dot {
  position: absolute;
  top: 18px;
  right: 18px;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  opacity: .85;
}

.ev-card__shape {
  position: absolute;
  width: 90px;
  height: 90px;
  border-radius: 50%;
  bottom: -36px;
  right: -36px;
  opacity: .07;
  pointer-events: none;
  transition: opacity .2s;
}

.ev-card:hover .ev-card__shape { opacity: .14; }

.ev-card__time {
  display: inline-flex;
  align-items: center;
  gap: 5px;
  font-size: .7rem;
  font-weight: 800;
  color: #7A6E62;
  background: #F4EFE8;
  padding: 4px 10px;
  border-radius: 100px;
  align-self: flex-start;
  margin-bottom: 12px;
  margin-top: 2px;
  letter-spacing: .03em;
}

.ev-card__name {
  font-family: 'Playfair Display', serif;
  font-weight: 900;
  font-size: 1.3rem;
  color: #1A1108;
  line-height: 1.15;
  margin-bottom: 8px;
  letter-spacing: -.01em;
  padding-right: 20px;
}

.ev-card__desc {
  font-size: .82rem;
  font-weight: 300;
  color: #7A6E62;
  line-height: 1.6;
  flex-grow: 1;
  margin-bottom: 16px;
}

.ev-card__meta {
  border-top: 1.5px solid #E8DFD0;
  padding-top: 12px;
  display: flex;
  flex-direction: column;
  gap: 6px;
  margin-bottom: 14px;
}

.ev-card__row {
  display: flex;
  align-items: flex-start;
  gap: 7px;
  font-size: .78rem;
  font-weight: 500;
  color: #5C5248;
  line-height: 1.4;
}

.ev-card__row--org {
  color: #A09488;
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
}

.ev-card__status {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  align-self: flex-start;
  font-size: .65rem;
  font-weight: 800;
  letter-spacing: .07em;
  text-transform: uppercase;
  padding: 5px 12px;
  border-radius: 100px;
}

.ev-card__status--open {
  background: #E6FBF5;
  color: #00875F;
  border: 1.5px solid #B3F0DE;
}

.ev-card__status--ltd {
  background: #FDF0FD;
  color: #9C2D9C;
  border: 1.5px solid #F0BDEF;
}

.ev-card__more {
  font-size: .7rem;
  font-weight: 800;
  letter-spacing: .08em;
  text-transform: uppercase;
  color: #A09488;
}

.empty-state {
  margin-top: 4rem;
  border: 2px dashed #E8DFD0;
  border-radius: 24px;
  background: #fff;
  padding: 3rem 1.5rem;
  text-align: center;
  color: #7A6E62;
}

.empty-state h3 {
  font-family: 'Playfair Display', serif;
  margin: .7rem 0 .35rem;
  font-size: 1.8rem;
}

/* ASOCIACIONES */
.organizers-band {
  width: 100%;
  margin-top: 1rem;
  padding: 0 0 5rem;
}

.organizers-band__inner {
  width: 100%;
  background: linear-gradient(180deg, #fffdf9 0%, #f7efe4 100%);
  border-top: 2px solid #E8DFD0;
  border-bottom: 2px solid #E8DFD0;
  padding: 3.5rem 0;
}

.organizers-band__header {
  max-width: 1280px;
  margin: 0 auto 1.8rem;
  padding: 0 1.5rem;
}

.organizers-band__eyebrow {
  display: inline-block;
  font-size: .72rem;
  font-weight: 800;
  letter-spacing: .16em;
  text-transform: uppercase;
  color: #7A6E62;
  margin-bottom: .6rem;
}

.organizers-band__header h2 {
  font-family: 'Playfair Display', serif;
  font-size: clamp(2.2rem, 4vw, 4rem);
  line-height: .95;
  color: #1A1108;
  margin-bottom: .6rem;
}

.organizers-band__header p {
  max-width: 700px;
  font-size: 1rem;
  color: #7A6E62;
  line-height: 1.7;
}

.organizers-band__slider {
  position: relative;
  overflow: hidden;
  max-width: 1280px;
  margin: 0 auto;
  padding: 0 1.5rem;
}

.organizers__fade {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 90px;
  z-index: 2;
  pointer-events: none;
}

.organizers__fade--left {
  left: 1.5rem;
  background: linear-gradient(to right, #f7efe4 0%, rgba(247,239,228,0) 100%);
}

.organizers__fade--right {
  right: 1.5rem;
  background: linear-gradient(to left, #f7efe4 0%, rgba(247,239,228,0) 100%);
}

.organizers__track {
  display: flex;
  align-items: stretch;
  gap: 18px;
  width: max-content;
  animation: marqueeLogos 28s linear infinite;
}

@keyframes marqueeLogos {
  from { transform: translateX(0); }
  to { transform: translateX(-50%); }
}

.organizer-card {
  min-width: 230px;
  max-width: 230px;
  border-radius: 24px;
  background: rgba(255,255,255,.88);
  border: 2px solid #E8DFD0;
  padding: 1.2rem 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: .8rem;
  box-shadow: 0 12px 26px rgba(26,17,8,.05);
  text-decoration: none;
  color: inherit;
  transition: transform .2s ease, box-shadow .2s ease;
}

.organizer-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 34px rgba(26,17,8,.08);
}

.organizer-card__logo {
  width: 92px;
  height: 92px;
  border-radius: 22px;
  background: #fff;
  border: 2px solid #E8DFD0;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  padding: 8px;
}

.organizer-card__logo img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.organizer-card__name {
  text-align: center;
  font-size: .9rem;
  font-weight: 700;
  color: #40362e;
  line-height: 1.35;
  min-height: 2.4em;
  display: flex;
  align-items: center;
}

.organizer-card__ig {
  font-size: .8rem;
  font-weight: 800;
  color: #FF4D1C;
}

/* MODAL */
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity .22s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}

.modal-scale-enter-active,
.modal-scale-leave-active {
  transition: transform .25s ease, opacity .25s ease;
}

.modal-scale-enter-from,
.modal-scale-leave-to {
  transform: scale(.98);
  opacity: 0;
}

.event-modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(26,17,8,.55);
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
  border: 2px solid #E8DFD0;
  box-shadow: 0 30px 90px rgba(26,17,8,.22);
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
  background: #1A1108;
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 3;
}

.event-modal__header {
  --accent: #FF4D1C;
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
  line-height: .95;
}

.event-modal__day-date {
  font-size: .82rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: .14em;
  opacity: .82;
}

.event-modal__status {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: .75rem;
  font-weight: 800;
  letter-spacing: .08em;
  text-transform: uppercase;
  padding: 10px 16px;
  border-radius: 100px;
  background: rgba(255,255,255,.16);
  border: 1.5px solid rgba(255,255,255,.28);
  color: #fff;
}

.event-modal__content {
  padding: 2rem;
  display: grid;
  grid-template-columns: 1.5fr .9fr;
  gap: 24px;
}

.event-modal__time {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  font-size: .78rem;
  font-weight: 800;
  color: #7A6E62;
  background: #F4EFE8;
  padding: 7px 12px;
  border-radius: 100px;
  margin-bottom: 1rem;
}

.event-modal__title {
  font-family: 'Playfair Display', serif;
  font-size: clamp(2rem, 4vw, 3.4rem);
  line-height: .95;
  color: #1A1108;
  margin-bottom: 1rem;
}

.event-modal__desc {
  font-size: 1rem;
  color: #6D6258;
  line-height: 1.8;
  margin-bottom: 1.6rem;
}

.event-info-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 16px;
}

.event-info-card {
  border: 1.5px solid #E8DFD0;
  background: #fff;
  border-radius: 18px;
  padding: 1rem;
}

.event-info-card__label {
  display: block;
  font-size: .7rem;
  font-weight: 800;
  letter-spacing: .12em;
  text-transform: uppercase;
  color: #A09488;
  margin-bottom: .65rem;
}

.event-info-card__value {
  display: flex;
  align-items: flex-start;
  gap: 8px;
  font-size: .92rem;
  color: #433a32;
  line-height: 1.5;
  font-weight: 600;
}

.event-info-card__value--link a {
  color: #FF4D1C;
  font-weight: 800;
  word-break: break-word;
  text-decoration: none;
}

.event-aside-card {
  background: #fff;
  border: 1.5px solid #E8DFD0;
  border-radius: 22px;
  padding: 1.2rem;
  position: sticky;
  top: 0;
}

.event-aside-card__eyebrow {
  display: inline-block;
  font-size: .7rem;
  font-weight: 800;
  letter-spacing: .14em;
  text-transform: uppercase;
  color: #A09488;
  margin-bottom: .8rem;
}

.event-aside-card__title {
  font-family: 'Playfair Display', serif;
  font-size: 1.55rem;
  line-height: 1.1;
  margin-bottom: .8rem;
  color: #1A1108;
}

.event-aside-card__text {
  font-size: .92rem;
  line-height: 1.7;
  color: #6D6258;
  margin-bottom: 1rem;
}

.event-aside-card__button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: #FF4D1C;
  color: #fff;
  padding: 12px 16px;
  border-radius: 100px;
  font-size: .82rem;
  font-weight: 800;
  letter-spacing: .04em;
  text-decoration: none;
}

.event-aside-card__empty {
  font-size: .85rem;
  color: #A09488;
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
  border-top: 1px solid rgba(255,255,255,.06);
}

.su-footer__brand {
  display: flex;
  flex-direction: column;
  gap: .4rem;
}

.footer-mark {
  font-family: 'Playfair Display', serif;
  font-size: 2.2rem;
  font-weight: 900;
  color: #FDF8F0;
  letter-spacing: -.02em;
}

.footer-mark em {
  font-style: normal;
  color: var(--r);
}

.su-footer__tag {
  font-size: .82rem;
  font-weight: 500;
  color: rgba(253,248,240,.62);
  line-height: 1.6;
  text-transform: uppercase;
  letter-spacing: .06em;
}

.su-footer__credits {
  display: flex;
  align-items: center;
  gap: .55rem;
  font-size: .92rem;
  color: rgba(253,248,240,.72);
}

.su-footer__credits a {
  color: #fff;
  font-weight: 800;
  text-decoration: none;
  border-bottom: 1px solid rgba(255,255,255,.3);
}

.su-footer__credits a:hover {
  color: #FFB39D;
  border-bottom-color: #FFB39D;
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

  .stat { padding: 0 1.5rem; }

  .agenda-controls {
    grid-template-columns: 1fr;
  }

  .legend-section__inner {
    grid-template-columns: 1fr;
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

  .heading__semana {
    font-size: clamp(3rem, 14vw, 5.2rem);
  }

  .heading__u {
    font-size: clamp(3.8rem, 16vw, 6rem);
    margin-left: .04em;
    margin-right: .02em;
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

  .agenda-controls-wrap {
    padding: 1.25rem 1rem 0;
  }

  .agenda {
    padding: 0 1rem 5rem;
  }

  .day-section {
    margin-top: 2.5rem;
  }

  .day-band {
    padding: 1.2rem 1.25rem;
  }

  .day-band__index {
    display: none;
  }

  .cards-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .event-modal-overlay {
    align-items: center;
    padding: .75rem;
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

  .organizer-card {
    min-width: 190px;
    max-width: 190px;
  }
}

@media (max-width: 640px) {
  .hero__content {
    padding: 4rem 1rem 1.5rem;
  }

  .hero__eyebrow {
    gap: 8px;
    font-size: .65rem;
    letter-spacing: .14em;
    margin-bottom: 1.2rem;
  }

  .eyebrow__line {
    max-width: 28px;
  }

  .heading__semana {
    font-size: clamp(2.7rem, 16vw, 4.2rem);
  }

  .heading__u {
    font-size: clamp(3.5rem, 18vw, 5rem);
  }

  .heading__year {
    font-size: clamp(1.5rem, 7vw, 2.2rem);
    padding-bottom: .15em;
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
    font-size: .95rem;
    line-height: 1.65;
    margin-bottom: 1.8rem;
  }

  .hero__scroll {
    font-size: .72rem;
  }

  .hero__stats {
    margin-top: 2rem;
    padding: .8rem;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
  }

  .stat {
    background: rgba(255,255,255,.04);
    border-radius: 14px;
    padding: 1rem .5rem;
  }

  .stat__n {
    font-size: 2rem;
  }

  .stat__l {
    font-size: .62rem;
    text-align: center;
  }

  .cards-grid {
    grid-template-columns: 1fr;
  }

  .ev-card {
    padding: 20px 18px 16px;
  }

  .ev-card__name {
    font-size: 1.2rem;
  }

  .organizers-band__header {
    padding: 0 1rem;
  }

  .organizers-band__slider {
    padding: 0 1rem;
  }

  .organizers__fade--left {
    left: 1rem;
  }

  .organizers__fade--right {
    right: 1rem;
  }

  .organizer-card {
    min-width: 165px;
    max-width: 165px;
    padding: .95rem .8rem;
  }

  .organizer-card__logo {
    width: 72px;
    height: 72px;
  }

  .organizer-card__name {
    font-size: .78rem;
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
    font-size: .75rem;
  }
}
</style>