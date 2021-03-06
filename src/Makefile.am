bin_PROGRAMS		= ffvademo
noinst_LTLIBRARIES	= libffva.la

# -----------------------------------------------------------------------------
# --- FFmpeg decoder module                                                 ---
# -----------------------------------------------------------------------------

if ENABLE_BUILTIN_FFMPEG
FFMPEG_CFLAGS	= \
	-I$(top_srcdir)/ext/ffmpeg/upstream \
	-I$(top_builddir)/ext/ffmpeg/objs
FFMPEG_LIBS	= $(top_builddir)/ext/ffmpeg/libffva_ffmpeg.la
else
FFMPEG_CFLAGS	= $(LIBAVUTIL_CFLAGS) $(LIBAVFORMAT_CFLAGS) $(LIBAVCODEC_CFLAGS)
FFMPEG_LIBS	= $(LIBAVUTIL_LIBS) $(LIBAVFORMAT_LIBS) $(LIBAVCODEC_LIBS)
endif

libffva_cflags = \
	$(FFMPEG_CFLAGS)	\
	$(LIBVA_CFLAGS)		\
	$(NULL)

libffva_libs = \
	$(FFMPEG_LIBS)		\
	$(LIBVA_LIBS)		\
	-lpthread		\
	$(NULL)

libffva_source_c = \
	ffmpeg_utils.c		\
	ffvadecoder.c		\
	ffvadisplay.c		\
	ffvafilter.c		\
	ffvarenderer.c		\
	ffvasurface.c		\
	vaapi_utils.c		\
	$(NULL)

libffva_source_h = \
	egl_compat.h		\
	ffmpeg_compat.h		\
	ffmpeg_utils.h		\
	ffvadecoder.h		\
	ffvadisplay.h		\
	ffvadisplay_priv.h	\
	ffvafilter.h		\
	ffvarenderer.h		\
	ffvarenderer_priv.h	\
	ffvasurface.h		\
	vaapi_compat.h		\
	vaapi_utils.h		\
	$(NULL)

libffva_source_drm_c = ffvarenderer_drm.c
libffva_source_drm_h = ffvarenderer_drm.h
libffva_source_x11_c = ffvarenderer_x11.c
libffva_source_x11_h = ffvarenderer_x11.h
libffva_source_egl_c = ffvarenderer_egl.c
libffva_source_egl_h = ffvarenderer_egl.h

if USE_DRM
libffva_source_c		+= $(libffva_source_drm_c)
libffva_source_h		+= $(libffva_source_drm_h)
libffva_cflags			+= $(LIBVA_DRM_CFLAGS) $(DRM_CFLAGS)
libffva_libs			+= $(LIBVA_DRM_LIBS) $(DRM_LIBS)
endif

if USE_X11
libffva_source_c		+= $(libffva_source_x11_c)
libffva_source_h		+= $(libffva_source_x11_h)
libffva_cflags			+= $(LIBVA_X11_CFLAGS) $(X11_CFLAGS)
libffva_libs			+= $(LIBVA_X11_LIBS) $(X11_LIBS)
endif

if USE_EGL
libffva_source_c		+= $(libffva_source_egl_c)
libffva_source_h		+= $(libffva_source_egl_h)
libffva_cflags			+= $(EGL_CFLAGS) $(GLAPI_CFLAGS)
libffva_libs			+= $(EGL_LIBS) $(GLAPI_LIBS)
endif

libffva_la_SOURCES		= $(libffva_source_c)
libffva_la_CFLAGS		= $(libffva_cflags)
libffva_la_LIBADD		= $(libffva_libs)

ffvademo_source_c		= ffvademo.c
ffvademo_SOURCES		= $(ffvademo_source_c)
ffvademo_CFLAGS			= $(libffva_cflags)
ffvademo_LDADD			= libffva.la

EXTRA_DIST = \
	$(libffva_source_c)	\
	$(libffva_source_h)	\
	$(libffva_source_drm_c)	\
	$(libffva_source_drm_h)	\
	$(libffva_source_x11_c)	\
	$(libffva_source_x11_h)	\
	$(libffva_source_egl_c)	\
	$(libffva_source_egl_h)	\
	$(NULL)

# Extra clean files so that maintainer-clean removes *everything*
MAINTAINERCLEANFILES = Makefile.in
